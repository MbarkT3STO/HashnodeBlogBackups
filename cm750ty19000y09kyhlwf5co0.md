---
title: "Cryptography: Vigenère Cipher"
datePublished: Fri Feb 14 2025 17:06:54 GMT+0000 (Coordinated Universal Time)
cuid: cm750ty19000y09kyhlwf5co0
slug: cryptography-vigenere-cipher
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1739551436306/f5409e7b-ad1e-4be9-8eed-93bb77032a05.jpeg
tags: algorithms, cryptography

---

The **Vigenère Cipher** is a classical encryption technique that extends the **Caesar Cipher** by using a key to shift letters in a cyclic manner. Unlike the Caesar Cipher, which uses a single shift value, the Vigenère Cipher applies multiple shifts based on the letters of a given keyword. This makes it more resistant to simple frequency analysis.

In this tutorial, we will explain how the Vigenère Cipher works, its mathematical foundation, and how to implement it in **C#** and **Python**.

## How Does the Vigenère Cipher Work?

The **Vigenère Cipher** encrypts text by applying a series of Caesar shifts determined by a **keyword**. Each letter of the keyword corresponds to a shift value, which is then applied cyclically to the plaintext.

### **Encryption Process**

1. Write the plaintext message.
    
2. Repeat the keyword until it matches the length of the plaintext.
    
3. Convert each letter of the plaintext and keyword to a numerical value (A=0, B=1, ..., Z=25).
    
4. Add the corresponding values (mod 26) to get the ciphertext.
    
5. Convert the resulting numbers back to letters.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739552390696/a72df883-c612-4c44-945f-35f4ed0e08b9.png align="center")

### **Decryption Process**

1. Convert the ciphertext and keyword to their respective numerical values.
    
2. Subtract the keyword value from the ciphertext value (mod 26) to retrieve the original letter.
    
3. Convert the resulting numbers back to letters.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739552543987/7345cbe1-115d-4cee-8cd9-486229bc3a12.png align="center")

#### **Example Encryption**

Let's encrypt **"HELLO"** with the keyword **"KEY"**.

| Plaintext | H | E | L | L | O |
| --- | --- | --- | --- | --- | --- |
| Key | K | E | Y | K | E |
| Shift | 10 | 4 | 24 | 10 | 4 |
| Encrypted | R | I | J | V | S |

Thus, **"HELLO"** becomes **"RIJVS"**.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1739552754372/47ce12c5-d89b-4e34-b64f-3a4026a195ef.png align="center")

## **Implementation in C#**

Let's implement both encryption and decryption in **C#**.

```csharp
using System;

class VigenereCipher
{
    static string Encrypt(string text, string key)
    {
        string result = "";
        int keyIndex = 0;
        key = key.ToUpper();

        foreach (char c in text.ToUpper())
        {
            if (char.IsLetter(c))
            {
                int shift = key[keyIndex] - 'A';
                char encryptedChar = (char)(((c - 'A' + shift) % 26) + 'A');
                result += encryptedChar;
                keyIndex = (keyIndex + 1) % key.Length;
            }
            else
            {
                result += c;
            }
        }
        return result;
    }

    static string Decrypt(string text, string key)
    {
        string result = "";
        int keyIndex = 0;
        key = key.ToUpper();

        foreach (char c in text.ToUpper())
        {
            if (char.IsLetter(c))
            {
                int shift = key[keyIndex] - 'A';
                char decryptedChar = (char)(((c - 'A' - shift + 26) % 26) + 'A');
                result += decryptedChar;
                keyIndex = (keyIndex + 1) % key.Length;
            }
            else
            {
                result += c;
            }
        }
        return result;
    }

    static void Main()
    {
        string plaintext = "HELLO";
        string key = "KEY";
        string encrypted = Encrypt(plaintext, key);
        string decrypted = Decrypt(encrypted, key);
        Console.WriteLine($"Encrypted: {encrypted}");
        Console.WriteLine($"Decrypted: {decrypted}");
    }
}
```

### **Output**

```python
Encrypted: RIJVS
Decrypted: HELLO
```

## **Implementation in Python**

Here's how we can implement the Vigenère Cipher in **Python**.

```python
def vigenere_encrypt(plaintext, key):
    key = key.upper()
    encrypted_text = ""
    key_index = 0

    for char in plaintext.upper():
        if char.isalpha():
            shift = ord(key[key_index]) - ord('A')
            encrypted_char = chr(((ord(char) - ord('A') + shift) % 26) + ord('A'))
            encrypted_text += encrypted_char
            key_index = (key_index + 1) % len(key)
        else:
            encrypted_text += char
    
    return encrypted_text


def vigenere_decrypt(ciphertext, key):
    key = key.upper()
    decrypted_text = ""
    key_index = 0

    for char in ciphertext.upper():
        if char.isalpha():
            shift = ord(key[key_index]) - ord('A')
            decrypted_char = chr(((ord(char) - ord('A') - shift + 26) % 26) + ord('A'))
            decrypted_text += decrypted_char
            key_index = (key_index + 1) % len(key)
        else:
            decrypted_text += char
    
    return decrypted_text


# Test the functions
plaintext = "HELLO"
key = "KEY"
ciphertext = vigenere_encrypt(plaintext, key)
decrypted_text = vigenere_decrypt(ciphertext, key)

print("Encrypted:", ciphertext)
print("Decrypted:", decrypted_text)
```

### **Output**

```python
Encrypted: RIJVS
Decrypted: HELLO
```

## **Security Considerations**

* The **Vigenère Cipher is vulnerable to cryptanalysis** (e.g., Kasiski examination) if the keyword is short or reused.
    
* Modern cryptographic algorithms (e.g., **AES**) are much more secure and recommended for real-world applications.