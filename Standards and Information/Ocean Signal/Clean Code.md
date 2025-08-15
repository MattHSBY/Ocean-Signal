# Clean Code

The importance of Clean Code cannot be overstated.
Clean Code makes for code that is easy to read and understand, and therefore maintain.

## What is Meant by Clean Code?

"Clean Code" is a term that refers to code that is easy to read, understand, maintain, and modify³. It's not a set of strict rules, but a set of principles that guide developers in writing code². Here are some key characteristics of clean code:

1. **Understandability**: The code can be immediately understood by any experienced developer². The task or role of each class, function, method, and variable is immediately understandable².

2. **Maintainability**: Code is easy to modify if it can be adapted and extended. This also makes it easier to correct errors in the code².

3. **Meaningful Names**: Do not use comments to explain why a variable is used. If a name requires a comment, then you should take your time to rename that variable instead of writing a comment¹.

4. **Avoid Disinformation**: Be careful about words that mean something specific. Do not refer to a grouping of accounts as accountList unless its type is actually a List¹.

5. **Avoid Noise Words**: Noise words are the words that do not offer any additional information about the variable. They are redundant and should be removed¹.

6. **Simplicity**: Write code as simply as possible. You should avoid making your code unnecessarily complex².

These principles were largely popularized by Robert Cecil Martin in his book "Clean Code: A Handbook of Agile Software Craftsmanship"². The goal of writing clean code is to make the code easy to read for other developers, which in turn makes it easier to maintain and extend¹²³.

Source: Conversation with Bing, 08/11/2023
(1) What is Clean Code? | Sonar - SonarSource. https://www.sonarsource.com/solutions/clean-code/.
(2) Clean code: principles, advantages and examples - IONOS. https://www.ionos.com/digitalguide/websites/web-development/clean-code-principles-advantages-and-examples/.
(3) Clean Code Explained – A Practical Introduction to Clean Coding for .... https://www.freecodecamp.org/news/clean-coding-for-beginners/.

## A Real World Example

The following example is taken from the _Beacon Final Test_ application, which is a rich seam of examples of poor coding practice.

### Example of Unclean Code

- Notice how unhelpful the comments are in this example.
  - The comments are unnecessarily ceremonial, and state what the code does rather than why it is doing it.
  - If the code were written to be more readable, this would be completely redundant.
  - The comments clutter the method and make it harder to follow the program flow. Clean code should be self-explanatory.
- Notice how a relatively simple method spans multiple screens, making it impossible to take in the details in a single look.
  - Methods should be short and fit on a single screen or less, so that the full method can be viewed all together.
  - White space and comments can do more harm than good and should be used wisely.
- Notice how this code uses an _imperative style_ which focuses on _how_ rather than _what_.
  - The details obscure the intent.
- There is a hard-coded literal string which is critical to correct operation.
  This code will break if the executable name ever changes.

```csharp
        /*******************************************************************************************
         * Check if this software is already running
         ******************************************************************************************/
        static bool IsAlreadyRunning()
        {
            int count = 0;

            for (int tries = 0; tries < 2; ++tries)
            {
                /*
                 * Store all running process in the system
                 */
                Process[] runingProcess = Process.GetProcesses();
                count = 0;
                for (int i = 0; i < runingProcess.Length; ++i)
                {
                    /*
                     * If we find an instance of "BeaconFinalTest" increment count
                     * so we can work out if more than one is running
                     */
                    if (runingProcess[i].ProcessName == "BeaconFinalTest")
                    {
                        count++;
                    }

                    if (count == 2)
                    {
                        /*
                         * If this is the first try then try again in 2 seconds in case the previous process
                         * is taking a little too long to die; and we will check again, otherwise just break out of the loop as we
                         * will not be retrying again.
                         */
                        if (tries == 0)
                        {
                            Thread.Sleep(2000);
                        }

                        break;
                    }
                }

                /*
                 * Only one instance is running (this one), so we are good to go
                 */
                if (count == 1)
                {
                    return false;
                }
            }

            /*
             * Still 2 running after second attempt! we need to return true so this instance
             * gets aborted
             */
            if (count == 2)
            {
                return true;
            }

            /*
             * We can't actually get here as count will not go above 2, but the compiler does not know
             * that, so we return false here just to keep it happy
             */
            return false;
        }
```

### Another Look With Comments Removed

- With all comments and unnecessary white-space removed, the code is still just about a whole screen long.
- There are 4 levels of nesting; the logic is almost _impenetrable_.

```csharp
        static bool IsAlreadyRunning()
        {
            int count = 0;

            for (int tries = 0; tries < 2; ++tries)
            {
                Process[] runingProcess = Process.GetProcesses();
                count = 0;
                for (int i = 0; i < runingProcess.Length; ++i)
                {
                    if (runingProcess[i].ProcessName == "BeaconFinalTest")
                    {
                        count++;
                    }
                    if (count == 2)
                    {
                        if (tries == 0)
                        {
                            Thread.Sleep(2000);
                        }

                        break;
                    }
                }
                if (count == 1)
                {
                    return false;
                }
            }
            if (count == 2)
            {
                return true;
            }
            return false;
        }
```

### Improved Clean Code

- Notice that the method fits on less than half a screen
  - It is easy to see and comprehend the whole method at a glance.
- Notice that comments are limited to a statement of intent.
  - Spurious comments and white space are absent in the method body.
  - "how it works" comments are unnecessary beacuse the code is simple to understand.
- Notice that the code uses a _declaritive style_ which focuses on _what_ rather than _how_
  - A LINQ query is used to select data rather than iterating over it in a for-loop.
- Notice that the logic is simple and the level of nesting is minimal.
  - Any person reading this code would be able to quickly understand what it does, ieven if they had never seen a LINQ comprehension query before.

```csharp
        /// <summary>
        ///     Checks whether the application is already running in another process.
        ///     If there is more than one instance and they persist for at least 2 seconds,
        ///     then that is considered to be positive indication of a duplicate instance.
        /// </summary>
        /// <returns><c>true</c> if a duplicate instance is running, <c>false</c> otherwise.</returns>
        [SuppressMessage("ReSharper", "PossibleMultipleEnumeration",
            Justification = "Intentional re-enumeration of processes to check for persistence over time")]
        private static bool ImprovedIsAlreadyRunning()
        {
            var me = Process.GetCurrentProcess().ProcessName;
            var processQuery = from process in Process.GetProcesses()
                where process.ProcessName == me
                select process;
            if (processQuery.Count() <= 1)
                return false;
            Thread.Sleep(TimeSpan.FromSeconds(2));
            return processQuery.Count() > 1;
        }
```