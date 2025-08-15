# Printing to the Zebra Printers

_Transcription of an email from Darren Nye, 31 October 2023_

Hi All, even though we are not doing this for existing applications it may be useful to do so in the future. I spent a little bit of time on it yesterday and successfully managed to get the printer to print. This method will allow us to send raw ZPL data through the print server without having to install the printers locally, or install drivers etc. I thought I would write it down and then decided it may be of use to you all...

First, a little bit of context; 

In the universal production tools, should you want to print to a zebra printer right now, you essentially define the printers ip address in the hardware profile, then create a Zebra 'measurement'.  Internally, this will customise the text data from the .zpl/.zpv file, then open a tcp connection to the printer, and squirt the raw printer commands down the pipe and out comes the label.

Though note that when Andrew and I looked yesterday, it appears that in beacon config, the printer is installed locally and then the same .zpl file is 'copied' to the printer, so we anticipate this should work in the same way even through the print server but you still need to install the printer to that machine!

The issue in the production tools of using the IP address of the print server with /zb03 (i.e., 172.16.6.2/zb03) will not work with TcpClient.Connect because TcpClient expects a TCP endpoint, and 172.16.6.2/zb03 is not a valid TCP endpoint. The /zb03 is specific to higher-level print sharing protocols and is not part of the TCP/IP address or port.

So, what about if we used .net PrintServer and printQueue classes, like this:

```csharp
        public void printPrettyPlease() 
        {
            // Connect to the print server
            PrintServer printServer = new PrintServer(@"\\os-ps-001");

            // Connect to the specific print queue
            PrintQueue printQueue = new PrintQueue(printServer, "zb03");

            // Read label in
            string textToPrint = File.ReadAllText("label.zpl");

            // Create a new print job
            PrintSystemJobInfo printJob = printQueue.AddJob();

            // Write the text content to the print job's stream
            using (Stream printStream = printJob.JobStream)
            {
                Byte[] bytes = System.Text.Encoding.UTF8.GetBytes(textToPrint);
                printStream.Write(bytes, 0, bytes.Length);
            }  // Stream is closed and job should start printing when exiting the using block
        }
```

They won't work, as I promptly found out yesterday. Though I could see the job appear in the printer job list it would disappear almost straight away...why?
Direct vs. Spooled Printing 

Direct Printing: When using TcpClient to connect directly to the printer on port 9100, we were sending raw Zebra Programming Language (ZPL) commands directly to the printer. This is a very low-level way of printing, bypassing any printer drivers or spoolers, it means we don't have to install any drivers, don't have to install any software, don't have to add a printer locally and so on!

Spooled Printing via Windows Print Server: When we attempted to use System.Printing, we were interacting with the Windows Spooler service. This is a higher-level service that manages printing documents for all printers installed on a computer or network. It's expecting standard document formats (like text or images) rather than raw printer commands, (which is likely why the print jobs were appearing in the queue but not actually printing, I think)

So, we need a way to interact with the print server and send the raw ZPL data to the printer, its a bit more involved but this demo program works! (note, its not exhaustive!!!)

```csharp
using System; 
using System.Runtime.InteropServices;

public class RawPrinterHelper
{
    [DllImport("winspool.Drv", SetLastError = true, CharSet = CharSet.Auto)]
    public static extern bool OpenPrinter(string szPrinter, out IntPtr hPrinter, IntPtr pd);

    [DllImport("winspool.Drv", SetLastError = true)]
    public static extern bool ClosePrinter(IntPtr hPrinter);

    [DllImport("winspool.Drv", SetLastError = true, CharSet = CharSet.Auto)]
    public static extern bool StartDocPrinter(IntPtr hPrinter, int level, [In, MarshalAs(UnmanagedType.LPStruct)] DOCINFO di);

    [DllImport("winspool.Drv", SetLastError = true)]
    public static extern bool EndDocPrinter(IntPtr hPrinter);

    [DllImport("winspool.Drv", SetLastError = true)]
    public static extern bool StartPagePrinter(IntPtr hPrinter);

    [DllImport("winspool.Drv", SetLastError = true)]
    public static extern bool EndPagePrinter(IntPtr hPrinter);

    [DllImport("winspool.Drv", SetLastError = true)]
    public static extern bool WritePrinter(IntPtr hPrinter, IntPtr pBytes, int dwCount, out int dwWritten);

    [StructLayout(LayoutKind.Sequential, CharSet = CharSet.Auto)]
    public class DOCINFO
    {
        public string pDocName;
        public string pOutputFile;
        public string pDataType;
    }

    public static bool SendBytesToPrinter(string szPrinterName, IntPtr pBytes, int dwCount)
    {
        IntPtr hPrinter;
        DOCINFO di = new DOCINFO();
        bool bSuccess = false;
        di.pDocName = "Raw Document";
        di.pDataType = "RAW";

        if (OpenPrinter(szPrinterName.Normalize(), out hPrinter, IntPtr.Zero))
        {
            if (StartDocPrinter(hPrinter, 1, di))
            {
                if (StartPagePrinter(hPrinter))
                {
                    int dwWritten;
                    bSuccess = WritePrinter(hPrinter, pBytes, dwCount, out dwWritten);
                    EndPagePrinter(hPrinter);
                }
                EndDocPrinter(hPrinter);
            }
            ClosePrinter(hPrinter);
        }
        return bSuccess;
    }

    public static void Main()
    {
        byte[] zplData = System.IO.File.ReadAllBytes("path/to/your.zpv");

        IntPtr pUnmanagedBytes = Marshal.AllocCoTaskMem(zplData.Length);
        Marshal.Copy(zplData, 0, pUnmanagedBytes, zplData.Length);

        bool success = SendBytesToPrinter(@"\\os-ps-001\zb03", pUnmanagedBytes, zplData.Length);

        if (success)
        {
            Console.WriteLine("The print job was sent successfully!");
        }
        else
        {
            Console.WriteLine("There was an error sending the print job.");
        }

        Marshal.FreeCoTaskMem(pUnmanagedBytes);
    }
}
```

What is actually happening here?

We are using Platform Invocation Services (P/Invoke) to call unmanaged code from the Windows API for printing. Specifically, it uses functions from the winspool.drv library to interact with the Windows print spooler, but in a way that lets you send raw data directly to the printer.

1.	OpenPrinter: Opens a handle to the printer.
2.	StartDocPrinter: Starts a new print job.
3.	StartPagePrinter: Starts a new page for that print job.
4.	WritePrinter: Writes the raw data (your ZPL commands) to the print job.
5.	EndPagePrinter: Ends the page.
6.	EndDocPrinter: Ends the print job and initiates actual printing.
7.	ClosePrinter: Closes the handle to the printer.

There may be a better way using the .net stuff exclusively but I could not find a way in the short time I spent...
