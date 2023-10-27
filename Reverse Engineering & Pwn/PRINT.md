This challenge provides you with a Python code that you need to correct to make it work correctly to obtain the flag.

Original source code:
```python
def sixfour(aha):
    # aha.insert(7, '4')
    # aha.insert(15, '2')
    # aha.insert(23, '0')
    # aha.insert('31', '5')
    # aha = ' '.join(aha)

    aha_sixfour = aha.encode("asci")
    naja_boy = base64.b32decode(aha_sixfour)
    naja = naja_b.decode("asci")

    printf("flag{" + str(naja) + "}")


def rolling(realText, step):
    out = []
    cryptText = []

    uppercase = [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
    ]
    lowercase = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
    ]

    for eachLetter in realText:
        if eachLetter in uppercase:
            index = uppercase.index(eachLetter)
            crypting = (index + step) % 25
            cryptText.append(crypting)
            newLetter = uppercase[crypting]
            out.append(newLetter)
        elif eachLetter in lowercase:
            index = lowercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = lowercase[crypting]
            out.append(newLetter)
    out.append("=")
    sixfour(out)


print("========== Welcome to PRINT ==========")
msg = "UNMtFqHTgY" + "rGATTAMqGDFUAH" + "eFATUqXfVKUsFgL"
n = "0"
rolling(msg, num)
```

So let's analyze the code starting with the `rolling` function.
```python
def rolling(realText, step):
    out = []
    cryptText = []

    uppercase = [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
    ]
    lowercase = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
    ]

    for eachLetter in realText:
        if eachLetter in uppercase:
            index = uppercase.index(eachLetter)
            crypting = (index + step) % 25 # <= This is problem.
            cryptText.append(crypting)
            newLetter = uppercase[crypting]
            out.append(newLetter)
        elif eachLetter in lowercase:
            index = lowercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = lowercase[crypting]
            out.append(newLetter)
    out.append("=")
    sixfour(out)
```

This function is shifting the alphabetic characters like the Caesar Cipher, so the problem that I comment in the code above is that in the alphabet A-Z have 26 characters, which means the index of the list is 0–25, but if the original code were `modulo` by 25, the result would be 0–24 (`Z` is never the result).  
So I tried to edit this section from `25` to `26`.
```python
def rolling(realText, step):
    out = []
    cryptText = []

    uppercase = [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
    ]
    lowercase = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
    ]

    for eachLetter in realText:
        if eachLetter in uppercase:
            index = uppercase.index(eachLetter)
            crypting = (index + step) % 26 # <= Fix to 26
            cryptText.append(crypting)
            newLetter = uppercase[crypting]
            out.append(newLetter)
        elif eachLetter in lowercase:
            index = lowercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = lowercase[crypting]
            out.append(newLetter)
    out.append("=")
    sixfour(out)
```

This would be work, maybe not?

Let's go to the next section.
```python
print("========== Welcome to PRINT ==========")
msg = "UNMtFqHTgY" + "rGATTAMqGDFUAH" + "eFATUqXfVKUsFgL"
n = "0" # <= This is problem
rolling(msg, num)
```

As you can see, `n` is a string and `num` is not defined yet, so I figured out that maybe I should change `n` to `num` as an assign value to `0`. So the code will look like this:
```python
print("========== Welcome to PRINT ==========")
msg = "UNMtFqHTgY" + "rGATTAMqGDFUAH" + "eFATUqXfVKUsFgL"
num = 0 # <= Fix this line
rolling(msg, num)
```

and finally, the `sixfour` function.
```python
def sixfour(aha):
    # aha.insert(7, '4')
    # aha.insert(15, '2')
    # aha.insert(23, '0')
    # aha.insert('31', '5') # <= This is problem
    # aha = ' '.join(aha) # <= This is problem

    aha_sixfour = aha.encode("asci")
    naja_boy = base64.b32decode(aha_sixfour)
    naja = naja_b.decode("asci")

    printf("flag{" + str(naja) + "}")
```

So I try to uncomment the code, but the trouble is that the `insert` method parameters in a list are `insert(index, data)`, but `aha.insert('31', '5')` first argument is string, so I change to `31` without a single quote, and the `join` method is used when you want to convert a list to a string, but the string in front of `join` is needed to be an empty string; if it's space like the original code, the result would be a string that is separated by space (like `A B C`).

The code will look like this:
```python
def sixfour(aha):
	aha.insert(7, "4")
	aha.insert(15, "2")
	aha.insert(23, "0")
	aha.insert(31, "5") # <= Change '31' to 31
	aha = ''
	.join(aha) # <= Change " " to ""

    aha_sixfour = aha.encode("asci")
    naja_boy = base64.b32decode(aha_sixfour)
    naja = naja_b.decode("asci")

    printf("flag{" + str(naja) + "}")
```

It looks like the `encode` method argument may misspell `asci` which should be `ascii`. Next, `base64` is the Python module, but we haven't imported it yet, so don't forget to import it. It uses `b32decode` which is a method to decode base32, but look closely; `aha` has been inserted by `0`.

Let's look the base32 table.  
![](./assets/Pasted%20image%2020230918005002.png)

As you can see, there is no `0` in base32 encoding, which means this is not base32, so I guess that it should be base64, so I change it to `b64decode` and the last line before the print flag.  
`naja_b` is misspelled again (maybe this coder has a skill issue, just kidding).  
Let's fix it, and this section code will look like this:
```python
def sixfour(aha):
	aha.insert(7, "4")
	aha.insert(15, "2")
	aha.insert(23, "0")
	aha.insert(31, "5")
	aha = ''.join(aha)

    aha_sixfour = aha.encode("ascii") # <= change to 'ascii'
    naja_boy = base64.b64decode(aha_sixfour) # <= change to b64decode
    naja = naja_boy.decode("ascii") # <= change to 'ascii'

    print("flag{" + str(naja) + "}") # <= change to print
```

The final code will look like this:
```python
import base64

def sixfour(aha):
	aha.insert(7, "4")
	aha.insert(15, "2")
	aha.insert(23, "0")
	aha.insert(31, "5")
	aha = ''.join(aha)

    aha_sixfour = aha.encode("ascii")
    naja_boy = base64.b64decode(aha_sixfour)
    naja = naja_boy.decode("ascii")

    print("flag{" + str(naja) + "}")

def rolling(realText, step):
    out = []
    cryptText = []

    uppercase = [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
    ]
    lowercase = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
    ]

    for eachLetter in realText:
        if eachLetter in uppercase:
            index = uppercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = uppercase[crypting]
            out.append(newLetter)
        elif eachLetter in lowercase:
            index = lowercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = lowercase[crypting]
            out.append(newLetter)
    out.append("=")
    sixfour(out)

print("========== Welcome to PRINT ==========")
msg = "UNMtFqHTgY" + "rGATTAMqGDFUAH" + "eFATUqXfVKUsFgL"
num = 0
rolling(msg, num)
```

Let's run the code.  
![](./assets/Pasted%20image%2020230918010101.png)

It says `byte 0xd3 is ordinal in range(128)` that means it cannot decode to `ascii` so I figured out that maybe it has rolled, but the problem is we don't know how many of those characters are shifted.

Look again at the `rolling` function; it is modulo by 26; that means if `num` of rolling is `26` the result would be the same, so that means we can brute force from `0` to `25` by using a for loop.  
I edited the code, and the code looks like this:
```python
import base64

def sixfour(aha):
	aha.insert(7, "4")
	aha.insert(15, "2")
	aha.insert(23, "0")
	aha.insert(31, "5")
	aha = ''.join(aha)

    aha_sixfour = aha.encode("ascii")
    naja_boy = base64.b64decode(aha_sixfour)
    naja = naja_boy.decode("ascii")

    print("flag{" + str(naja) + "}")

def rolling(realText, step):
    out = []
    cryptText = []

    uppercase = [
        "A",
        "B",
        "C",
        "D",
        "E",
        "F",
        "G",
        "H",
        "I",
        "J",
        "K",
        "L",
        "M",
        "N",
        "O",
        "P",
        "Q",
        "R",
        "S",
        "T",
        "U",
        "V",
        "W",
        "X",
        "Y",
        "Z",
    ]
    lowercase = [
        "a",
        "b",
        "c",
        "d",
        "e",
        "f",
        "g",
        "h",
        "i",
        "j",
        "k",
        "l",
        "m",
        "n",
        "o",
        "p",
        "q",
        "r",
        "s",
        "t",
        "u",
        "v",
        "w",
        "x",
        "y",
        "z",
    ]

    for eachLetter in realText:
        if eachLetter in uppercase:
            index = uppercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = uppercase[crypting]
            out.append(newLetter)
        elif eachLetter in lowercase:
            index = lowercase.index(eachLetter)
            crypting = (index + step) % 26
            cryptText.append(crypting)
            newLetter = lowercase[crypting]
            out.append(newLetter)
    out.append("=")
    sixfour(out)

print("========== Welcome to PRINT ==========")
msg = "UNMtFqHTgY" + "rGATTAMqGDFUAH" + "eFATUqXfVKUsFgL"
for i in range(26):
    try:
        rolling(msg, i)
    except:
        pass
```

I need to use try...except when the exception of `decode` method is throw error I want to pass to the next iteration and prevent program to be exited.  
![](./assets/Pasted%20image%2020230918011057.png)

We've got the flag!  
The flag result is in MD5 format, so you can answer in that format.  
CTT23{--MD5--}

My aspect of this challenge  
I think it should not be in the `Reverse Engineering & Pwn` category because this category needs to reverse or decompile the executable program and analyze it, but this challenge needs to fix the code to make it work again. I think it should be in the `Programming` category.

I wonder why the TCTT2023 CTF combined the `Reverse Engineering & Pwn` category together.

Finally, "WHY IS THIS CHALLENGE WORTH MUCH MORE THAN A 100 SCORE CHALLENGE THAT IS MORE DIFFICULT THAN THIS SH!T?"

This is my first Medium write-up post, so I apologize if I misunderstand the explanation, if the grammar is not correct, or if my wording displeases your reading experience.

Thank you for reading.
