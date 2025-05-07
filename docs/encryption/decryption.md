---
title: Step 2 - Decryption 
layout: default
parent: Notebook 3 – Encryption and Decryption 
---

## Decrypting Received Data

After downloading the encrypted data from the server, we need to decrypt it before we can use it for analysis. 

To simplify this process, we provide an implementation in a Python notebook. You can dowload the Jupyter Notebook here: [GitHub](). 

{EXPLAIN WHAT IS GOING ON HERE AND THAT IT IS COMPLEMENTARY TO THE ENCRYPTION IN UNITY}

```python
# Function to decrypt AES-CBC encrypted data
def decrypt_aes(encrypted_data, key, iv_length = 16):
    
    """
    Decrypts AES-CBC encrypted data and removes the padding using PKCS7.

    This function decrypts the provided encrypted data using the AES algorithm
    in CBC (Cipher Block Chaining) mode, then removes the padding that was 
    added during encryption using the PKCS7 padding scheme.

    Parameters:
    - encrypted_data (bytes): The encrypted data to be decrypted.
    - key (bytes): The AES encryption key (must be 16 bytes for AES-128).
    - iv_lengths (int): Lengths of random AES initialization vector (IV). 

    Returns:
    - bytes: The decrypted data with padding removed, i.e., the original plaintext.
    
    """
    
    # Extract the IV from the beginning of the encrypted data
    iv = encrypted_data[:iv_length]

    # Remove the IV from the encrypted data
    data = encrypted_data[iv_length:]
    
    # Initialize the cipher for decryption using AES-CBC mode with provided key and IV
    cipher = Cipher(algorithms.AES(key), modes.CBC(iv))
    decryptor = cipher.decryptor()
    
    # Decrypt the data
    decrypted_padded = decryptor.update(data) + decryptor.finalize()
    
    # Remove padding using PKCS7
    unpadder = padding.PKCS7(128).unpadder() #128 referrs to block size used by AES encryption
    decrypted_data = unpadder.update(decrypted_padded) + unpadder.finalize()
    
    return decrypted_data 
```



## Configuring and running Jupyter Notebook

**1. Configure encryption key**  
To use the decryption script, follow these steps:

1. Provide the **encryption key**. This must be the same values used for encryption. 
2. Specify the **filepath** to the folder containing the encrypted files. You also need to specify the **filepath** for the folder where you want the decrypted files to be saved.

![](../../assets/images/decryption1.png)

**2. Run and check code**  
After configuring the script, run the rest of the code to perform decryption.



When you follow the step-by-step tutorial using the Unity template, the decrypted data should look like this:

"This is test data for block: 1"

"This is test data for block: 2"