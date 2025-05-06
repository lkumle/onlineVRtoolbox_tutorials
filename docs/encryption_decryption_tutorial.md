---
title: Encryption and Decryption of Data
nav_order: 4
---

# 3. Encryption and Decryption of Data

Authors:   
last edited:  

citation:

---

**ADD force htpps**

### Overview:

In this tutorial, we will cover how to ensure data privacy during transmission and storage by using encryption.

Encryption is a technique used to convert readable information (plaintext) into an unreadable format (ciphertext) to protect it from unauthorized access. In this process, data is scrambled using a secret key. Without this key, the data appears as random noise, making it impossible to interpret. This ensures that only authorized individuals (e.g., researchers with the correct key) can access the original data.

Decryption is the reverse process, where ciphertext is transformed back into its original form using the same key used for encryption.

In this tutorial, we will be using **AES (Advanced Encryption Standard)**, a widely adopted symmetric encryption algorithm. Symmetric means that the same secret key is used for both encryption and decryption. Thanks to its popularity, implementations of AES are available in various programming frameworks, which makes it easy to use in your projects.

To use AES encryption correctly, we need:
- **Encryption Key**: A secret key used for both encryption and decryption. The security of the encryption relies on this key.
- **Initialization Vector (IV)**: A random value used to ensure that the same data encrypted multiple times will produce different ciphertexts each time, adding an extra layer of security. The IV is not secret and will therefore be transferred together with the experiment data. 

In this section, we will first explore how the data is encrypted within Unity. Then, we'll go over the decryption process once the encrypted data is downloaded from the server.

---
---

### 3.1 Encrypting Data in Unity

The easiest way to securely encrypt data within our Unity experiment is by utilizing C#’s built-in AES implementation via the `System.Security.Cryptography` namespace. We have implemented AES encryption in the `Encrypt()` function, which is part of the `ConnectionHandler.cs` script. 

To use the `Encrypt()` function, we our unity app needs access the key piece of information: the **encryption key**. This is implemented as follows: When establishing a connection, the server also queries the encryption key from the server. 




**To configure this:**
1. go to your server on pythonanythwere.com, specify the 
2. In the configuration section, modify the variables `encryptionKey` with your own key. 

Go to WEB tab and hit reload domain thing. otherwise can take a while to be online.

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Decryption/decryption1.png" alt="C# implementation" style="width: 1000%;">
</div>

---

<div style="background-color: #FDF1D3; border-left: 6px solid #F2AA84; padding: 10px;">
  <strong>Note:</strong> If you're just testing the overall pipeline, feel free to leave the keys as they are. You can always replace them with your own keys later!
</div>

---
  
**How to create an encryption key:**
- The key needs to have a lengths of 16, 24 or 32 bytes
- In our case, we specify the key as a string. The `Encrypt()` function then converts this string into a byte array using UTF-8 encoding. As such, any 16/24/32 character string using ASCII characters will result in a valid encryption key. 

Note that ASCII-based AES keys that are hardcoded in the server backend does not correspond the highest possible security standard (i.e., if the server is compromised the key is available). As a general note, no identifiable personal data should be transferred and stored from the unity app. Only deidentified reserch data. If you plan to transfer sensitive data, make sure to work with your university data protection team to ensure that the data transfer is secure enough to comply with insitutional guidelines. 

**How IVs are transferred**:
Randomly generated each time we are encrypting data. Then added to the beginning of the encrypted data. Trnsferred with the data. As said above, the IV is not secret and can be transferred with the data. 

That’s it! You’re ready to encrypt your data.

Here is a detailed information about the encrytpt function:

[add code of encrypt function]


---
---

### 3.2 Decrypting Received Data

After downloading the encrypted data from the server, we need to decrypt it before we can use it for analysis. To simplify this process, we provide an implementation in a Python notebook. You can dowload the Jupyter Notebook here: [LINK]. 

**Step 1: Decrypt data**  
To use the decryption script, follow these steps:

1. Provide the **encryption key**. This must be the same values used for encryption. 
2. Specify the **filepath** to the folder containing the encrypted files. You also need to specify the **filepath** for the folder where you want the decrypted files to be saved.

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Decryption/decryption2.png" alt="Notebook" style="width: 1000%;">
</div>

The IV is extracted automatically by the decryot function. After configuring the script, run the rest of the code to perform decryption.

**Step 2: Double check decryption**  
When you follow the step-by-step tutorial using the Unity template, the decrypted data should look like this:

"This is test data for block: 1"

"This is test data for block: 2"