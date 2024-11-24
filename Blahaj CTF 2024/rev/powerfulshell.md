# PowerfulShell

We are given this powershell script:
```
iex (-join ([System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("this really long base 64
string")).ToCharArray() | ForEach-Object {[char]([int]$_ -bxor [int][char]("1"))}))
```

This:
1. Decodes the Base-64 string into a byte array
2. Converts the byte array from Base-64 to UTF8 string
3. `.ToCharArray()` Converts the UTF8 string to a char array
4. Iterates over each character in the array and performs a bitwise XOR (-bxor) operation.
- Each character is converted to its ASCII integer value ([int]$_), XORed with the integer value of the character "1" ([int][char]("1")), and then cast back to a character ([char]).
- The result is a transformed character array.
5. `-join`: Combines the transformed character array back into a single string.
6. `iex`: stands for Invoke-Expression, which executes the resulting string as a PowerShell command.

Luckily for us, XOR is reversible, so we can get back the original string.

###Solve script

To run in powershell, ``powershell -executionpolicy bypass -File .\powershell_solve.ps1`

```
$base64 = "the really long base 64 string"

$decodedString = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($base64))

$originalString = ($decodedString.ToCharArray() | ForEach-Object {
    [char]([int]$_ -bxor [int][char]("1"))
}) -join ''


echo $originalString >> your_filepath/decode.txt
```

Running this once yields
```
echo "Please wait for us to calculate a flag..."; $secret = "b"; iex (-join ([System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("another long base 64 string")).ToCharArray() | ForEach-Object {[char]([int]$_ -bxor [int][char]("k"))}))
```

Because of time constraints + idk powershell + it works, instead of making a for loop or something sensible, so i just make a new .ps1 solve file, paste the new base 64 string in and replace the chara it is XORing with, then get the output. Rinse and repeat 24 times, to get 

`blahaj{p0w3rFu1L_ShEl1}`