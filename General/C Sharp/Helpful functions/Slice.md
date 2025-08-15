The ReadOnlySpan class defines a method "Slice":
```C#
[MethodImpl(MethodImplOptions.AggressiveInlining)]
public ReadOnlySpan<T> Slice(int start, int length)
{
  if ((ulong) (uint) start + (ulong) (uint) length > (ulong) (uint) this._length)
    ThrowHelper.ThrowArgumentOutOfRangeException();
  return new ReadOnlySpan<T>(ref Unsafe.Add<T>(this._reference, (IntPtr) (uint) start), length);
}
```
This method forms a slice out of the readonly span that begins at the specified index for a specified length.