# Encrypting Secrets with JS and Decrypting with Python
# A Concise Handbook

## Introduction - Navigating the Intricacies

Encrypting and decrypting user-provided secrets may appear deceptively simple when split between the front end and back end. 
However, the intricacies involved demand meticulous attention to detail. 
Even minor errors during encryption or decryption process can lead to significant issues, potentially resulting in failure.

In my exploration of this process, I delved into encryption and decryption using both Python and JavaScript.
Despite their apparent similarities, I encountered perplexing errors when attempting to encrypt in JS and decrypt in Python.
After identifying and rectifying these subtle mistakes, my goal is to share the solutions with you.

## Getting Started - Essential Tools

To embark on this encryption journey, I relied on the CryptoJS package for the front end, installable via npm:

```
npm install crypto-js
```

On the back end, the pyCryptodome Python package proved invaluable. 
Notably, PyCryptodome succeeded the PyCrypto package, offering self-contained cryptographic primitives. 
Installation is straightforward with pip:

```
pip install pycryptodome
```


## TL;DR: Quick Guide

If you're in a hurry for the code, here's a quick guide. 
On the front end, using JS, find the code on CodePen here (https://codepen.io/duskin/pen/MWLxZrx). 
The encryption process involves defining a secret key, payload, 
and utilizing AES encryption with a derived key and encryption options.

```
// Define the secret key and payload
var secretKey = "LefjQ2pEXmiy/nNZvEJ43i8hJuaAnzbA1Cbn1hOuAgA=";
var payload = "Hello";

// Convert the secret key to a format suitable for encryption
var derived_key = CryptoJS.enc.Base64.parse(secretKey);

// Initialize the initialization vector (IV) and encryption mode
var iv = CryptoJS.enc.Utf8.parse("1020304050607080");
var encryptionOptions = {
    iv: iv,
    mode: CryptoJS.mode.CBC
};

// Encrypt the payload using AES encryption with the derived key and encryption options
var test = CryptoJS.AES.encrypt(payload, derived_key, encryptionOptions).toString();
```

The result is the encrypted payload: "qvBJdcdH76P0R1xmk/mSKA==". 
To decrypt it in Python, run the provided script.

```
from base64 import b64decode
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

def decrypt(data: str) -> str:
    secret_key = "LefjQ2pEXmiy/nNZvEJ43i8hJuaAnzbA1Cbn1hOuAgA="
    iv = "1020304050607080"
    ciphertext = b64decode(data)
    derived_key = b64decode(secret_key)
    cipher = AES.new(derived_key, AES.MODE_CBC, iv.encode('utf-8'))
    decrypted_data = cipher.decrypt(ciphertext)
    return unpad(decrypted_data, 16).decode("utf-8")

decrypt("qvBJdcdH76P0R1xmk/mSKA==")
```

## The front side

CryptoJS supports a wide range of cryptographic algorithms, including symmetric key encryption algorithms like AES (Advanced Encryption Standard), 
hashing functions like SHA-256, and more. 
Its modular design allows developers to selectively include specific algorithms, minimizing the footprint of the library based on project requirements.
In the context of our encryption process, we leverage the robust AES encryption, a widely adopted symmetric algorithm, to securely encrypt text.

Setting up the front side is straightforward. 
Start by obtaining the encryption's secret key, securely stored as an environment variable. 
For the Initialization Vector (IV), a random 16-character string is generated. 
This random or pseudorandom value ensures unique ciphertexts for identical plaintexts, enhancing encryption security.

The IV is typically represented as a sequence of random bytes, demonstrated here with a 16-digit random number. 
At the process's end, the randomly generated IV combines with the encrypted payload for the backend's decryption.

```javascript
// Importing the CryptoJS library for encryption
import CryptoJS from "crypto-js";

// Function to generate a random 16-digit buffer
function random16String() {
  let result = '';
  for (let i = 0; i < 16; i++) {
    result += Math.floor(Math.random() * 10);
  }
  return String(result);
}

// Exported function for encrypting text
export function encrypt(text) {
  // Parsing the Base64-encoded secret key from environment variables
  var derived_key = CryptoJS.enc.Base64.parse(process.env.REACT_APP_SECRET_KEY);

  // Generating a random 16-digit string to be used as the initialization vector (IV)  
  var iv = CryptoJS.enc.Utf8.parse(random16String());

  // Encrypting the text using AES encryption in CBC mode with the derived key and random IV
  var payload = CryptoJS.AES.encrypt(text, derived_key, {
    iv: iv,
    mode: CryptoJS.mode.CBC,
  }).toString();

  // Combining the random 16-digit number and the encrypted payload
  return randomInt16 + payload;
}
```

## The back side

On the backend, I provide both encryption and decryption functions. 
The decryption function seamlessly works with the JavaScript encryption function from the front side.

The encryption function mirrors the one on the front side, employing Python's secrets.token_bytes 
to generate a random byte string for the Initialization Vector (IV). 
This IV, combined with the secret key, is used to create the ciphertext using the Crypto module.

The Crypto module encompasses a range of submodules, each designed to address specific cryptographic needs. 
These submodules include Crypto.Cipher, Crypto.Signature, Crypto.Hash, Crypto.Random, and more. 
Together, they form a cohesive toolkit for implementing cryptographic algorithms in Python.

The Crypto.Cipher submodule is integral for implementing symmetric key algorithms like AES (Advanced Encryption Standard and other block ciphers.

The pyCryptodome library, where the Crypto module resides, is a self-contained Python package that builds upon the original PyCrypto library. 

The Crypto helper class code is as follow:

```python
import secrets
from base64 import b64encode, b64decode

from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad


class Crypto:
    """A simple helper class for AES-256 encryption and decryption."""

    def __init__(self, key: bytes = None):
        """
        Initializes the class with a secret key.

        Args:
            key: A 32-byte secret key for encryption and decryption.
        """
        self.key = key

    def encrypt(self, data: str, iv: str = None) -> str:
        """
        Encrypts plaintext data and returns the base64 encoded ciphertext.

        Args:
            data: The plain text data to be encrypted.
            iv: The iv to use (Optional).

        Returns:
            The base64 encoded ciphertext.
        """
        data = pad(data.encode("utf-8"), 16)
        iv = iv or b64encode(secrets.token_bytes(16))[:AES.block_size]
        derived_key = b64decode(self.key)
        cipher = AES.new(derived_key, AES.MODE_CBC, iv)
        ciphertext = cipher.encrypt(data)
        return iv.decode("utf-8") + b64encode(ciphertext).decode("utf-8")

    def decrypt(self, data: str) -> str:
        """
        Decrypts base64 encoded ciphertext and returns the plain text.

        Args:
            data: The base64 encoded ciphertext to be decrypted.

        Returns:
            The plain text data.
        """
        iv, ciphertext = data[:AES.block_size], b64decode(data[AES.block_size:])
        derived_key = b64decode(self.key)
        cipher = AES.new(derived_key, AES.MODE_CBC, iv.encode('utf-8'))
        decrypted_data = cipher.decrypt(ciphertext)
        return unpad(decrypted_data, 16).decode("utf-8")
```

## Usage Example

The article includes a simple example showcasing the usage of the presented Crypto class for encryption and decryption. 
Replace 'my32bytesecretkey' with your actual secret key for practical implementation.

```python
# Example usage of the encryption and decryption functions
crypto = Crypto(key='my32bytesecretkey')  # Replace with your actual secret key

# Encrypting
plaintext = "Sensitive information"
encrypted_text = crypto.encrypt(plaintext)
print("Encrypted Text:", encrypted_text)

# Decrypting
decrypted_text = crypto.decrypt(encrypted_text)
print("Decrypted Text:", decrypted_text)
```

## Conclusion

This comprehensive guide aims to save developers valuable time by offering a easy solution 
for securely transferring encrypted data between the front and back end of applications. 
The code snippet simplifies the crucial process of encrypting and decrypting data, 
ensuring robust encryption practices for safeguarding sensitive information.

I trust that this resource enhances the security of your data transmissions and contributes 
to a smoother development experience. Feel free to iterate on the provided code to meet the specific requirements
of your projects. Happy coding!
