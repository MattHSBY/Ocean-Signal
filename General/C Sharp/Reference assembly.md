A reference assembly is a kind of .NET assembly (DLL) that contains only the public API metadata - types, methods, properties, events, etc.- but no actual method bodies or implementation code.
Its purpose is to compile against, not to run.

It's like a blueprint. For example, the method "GetHINSTANCE" in `System.Runtime.InteropServices` is not implemented in this definition, it simply is shown to "exist".

```C#
public static partial class Marshal
{
	[LibraryImport("QCall", EntryPoint = "MarshalNative_GetHINSTANCE")]
	[DllImport("QCall", EntryPoint = "MarshalNative_GetHINSTANCE")]
	private static extern IntPtr GetHINSTANCE(QCallModule m);

}
```