This challenge contains the "Reverse CTF.exe" executable program on Windows.

First, I checked file information uses `file` command
![[Pasted image 20230917170026.png]]

As you can see this program is .NET framework written by C# Language so I need Reverse Engineering tools for this C# program.

I use `dnSpy` for this challenge.
https://github.com/dnSpy/dnSpy

Let's check the program first.
When execute the application you will see this window box.
![[Pasted image 20230925005725.png]]

You need to find `Username` and `Password`, so I try to enter randomly character to textfield.
![[Pasted image 20230925005918.png]]

As I expected, the application says 'Login Fail'.

OK, let's reverse engineer this challenge.  
I find the `Main` function first, and the `Main` function only runs with class `Form1`.  
Next, I inspect the `Form1` class, and it looks like I found the username and password, but the password is encrypted.
![[Pasted image 20230925010309.png]]

Further analyze the `button1_Click` event.  
If the `flag` is true when `textBox1` (username) is equal to 'value', and the `value` is `admin`, then the username is `admin`.  
Next, `flag2` is true when `textBox2` (password) is equal to `text3`, but the `text3` is obtained from `Form1.DecryptDataWithAes(cipherText, keyBase, vectorBase)`.  
Therefore, we need to analyze the `DecryptDataWithAes` function.
![[Pasted image 20230925011504.png]]

Thus, we got the Decrypt function so I create the simple C# Console Application to test this function according to below cod.
`DecryptDataWithAes` in the application pass three arguments, `cipherText`, `keyBase` and `vectorBase`, therefore I bring 3 value from program to decrypt the `password`.
```cs
using System;
using System.Security.Cryptography;
class Program
{

    private static string DecryptDataWithAes(string cipherText, string keyBase64, string vectorBase64)
    {
        string result;
        using (Aes aes = Aes.Create())
        {
            aes.Key = Convert.FromBase64String(keyBase64);
            aes.IV = Convert.FromBase64String(vectorBase64);
            ICryptoTransform transform = aes.CreateDecryptor();
            byte[] buffer = Convert.FromBase64String(cipherText);
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
    static void Main(string[] args)
    {
        string keyBase = "yJ4L5ouh5D1n9Yj53S+GkEt/+OW/bn10MfC/opJjZ5g=";
        string vectorBase = "4ZttzHusKGsjzmdQA5+Viw==";
        string cipherText = "bR/dyVtoEuyudh4qoLWqkw==";
        Console.WriteLine("{0}", DecryptDataWithAes(cipherText, keyBase, vectorBase));
    }
}
```

![[Pasted image 20230925012110.png]]

We got the password, don't be hesitated, let's try to login the program with credentials that we got.
![[Pasted image 20230925012522.png]]

PAN PAKA PAN!!
Finally, we got the flag.

Fun fact: Actually, the flag that I censored is the password that we decrypted.