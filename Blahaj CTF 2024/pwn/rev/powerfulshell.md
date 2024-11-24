# PowerfulShell

We are given this powershell script:
```
iex (-join ([System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String("this really long base 64
string")).ToCharArray() | ForEach-Object {[char]([int]$_ -bxor [int][char]("1"))}))
```
