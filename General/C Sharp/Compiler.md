In order to run C# code, it must be "compiled".

A compiler is a software program that translates code written in a high-level programming language (in this case C#) into a low-level language, typically machine code or byte-code, that a computer's processor can understand and execute. Essentially, it acts as a translator between human-readable code and the language a computer understands directly.

For C#, code is first compiled into MSIL (Microsoft intermediate language), which is a CPU-independent set of instructions that can be efficiently converted to native code. Regardless of which compiler you use, the result is a managed module which is a standard 32-bit Windows portable executable (PE32) file, or a standard 64-bit Windows portable executable (PE32+) file that requires runtime to execute.
