This challenge contains the `CryptoLockerMe.exe` executable program on Windows and `flag.crypt`, the flag that encrypted.

flag.crypt
```
B1cssL8Z5ljeNSrl6/zY27hgvqzmh7o9CtHN7n0rJEo=
```

Let's check the file type of `CryptoLockerMe.exe` file type.
![](./assets/Pasted%20image%2020230925013501.png)

It's the .Net application or C# Application, so I use `dnSpy` for this challenge.
https://github.com/dnSpy/dnSpy

Let's launch the program.    
![](./assets/Pasted%20image%2020230925013702.png)

Wow, it very spook me, lol.
OK what happened when I click `Got it` button.    
![](./assets/Pasted%20image%2020230925013845.png)

Why, I need to pay you when I can reverse engineer this application to recover the flag.
Don't be hesitated. Let's reverse and inspect it.
![](./assets/Pasted%20image%2020230925014020.png)

So, I find `Main` function first and it called `Application.Run` with class `Form1`, then I analyze the `Form1` class next.
And look like we found the function that encrypt the our flag.

I focus on `File.WriteAllText(path3, Form1.ea(p, text, this.v)` that the program write the `flag.crypt`.
So the passed argument is `p`, `text`, `this.v`. Let's inspect the argument from first to last.

`p = File.ReadAllText(path2)`, thus `path2` is "flag,txt", so we may assumed that is the flag that we need before encrypted.

`text = File.ReadAllText(path)`, so `path` is "00000000.eky" that writen by `contents` obtained from `Form1.da(this.k, this.z, this.v)` passed by local class variable.

Let's find class variable.
![](./assets/Pasted%20image%2020230925020119.png)

```csharp
private static string k = "5zpQuV1qz/iYvKp2wMW1481zkdzxcCaAeBI+gzlrnVEzje4UqBeErIt8lRnr3oU0";

private static string z = "yJ4L5ouh5D1n9Yj53S+GkEt/+OW/bn10MfC/opJjZ5g=";

private static string v = "4ZttzHusKGsjzmdQA5+Viw==";
```

Next, I need to find `ea` and `da` which are the encrypt and decrypt function.

![](./assets/Pasted%20image%2020230925020635.png)

![](./assets/Pasted%20image%2020230925020649.png)

So, we got all data and function that we need to solve the challenge.
Let's try to decrypt in the C# Console App that we crafted.

```csharp
using System;
using System.Security.Cryptography;

class Program
{
    private static string da(string c, string z, string v)
    {
        string result;
        using (Aes aes = Aes.Create())
        {
            aes.Key = Convert.FromBase64String(z);
            aes.IV = Convert.FromBase64String(v);
            ICryptoTransform transform = aes.CreateDecryptor();
            byte[] buffer = Convert.FromBase64String(c);
            using (MemoryStream memoryStream = new MemoryStream(buffer))
            {
                using (CryptoStream cryptoStream = new CryptoStream(memoryStream, transform, CryptoStreamMode.Read))
                {
                    using (StreamReader streamReader = new StreamReader(cryptoStream))
                    {
                        result = streamReader.ReadToEnd();
                    }
                }
            }
        }
        return result;
    }
    private static string k = "5zpQuV1qz/iYvKp2wMW1481zkdzxcCaAeBI+gzlrnVEzje4UqBeErIt8lRnr3oU0";
    private static string z = "yJ4L5ouh5D1n9Yj53S+GkEt/+OW/bn10MfC/opJjZ5g=";
    private static string v = "4ZttzHusKGsjzmdQA5+Viw==";
    static void Main(string[] args)
    {
        string c = "B1cssL8Z5ljeNSrl6/zY27hgvqzmh7o9CtHN7n0rJEo=";
        string contents = da(k, z, v);
        string flag = da(c, contents, v);
        Console.WriteLine(flag);
    }
}
```

`File.WriteAllText(path3, Form1.ea(p, text, this.v)` remember this function that write the encrypt flag?
Therefore we can use benefit from `da` to decrypt the flag back to the plain text again.
So we called `da(c, contents, v)` same as `ea(p, text, this.v)` but we pass the cipher text instead of `p`.
Due to AES is Symmetric Encryption so we can use the same key and vector to decrypt the text.

![](./assets/Pasted%20image%2020230925021358.png)

PAN PAKA PAN!!
Finally, we got the flag.
