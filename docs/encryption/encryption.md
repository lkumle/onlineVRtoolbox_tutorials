---
title: Step 1 - Encryption in Unity
layout: default
parent: Notebook 3 – Encryption and Decryption 
---


## Encrypting Data in Unity

The easiest way to securely encrypt data within our Unity experiment is by utilizing C#’s built-in AES implementation via the `System.Security.Cryptography` namespace. 
We have implemented AES encryption in the `Encrypt()` function, which is part of the `ConnectionHandler.cs` script. 



To use the `Encrypt()` function, our unity app needs access the key piece of information: the **encryption key**. 


This is implemented as follows: 
When establishing a connection, the server also queries the encryption key from the server. 






{: .highlight}
If you're just testing the overall pipeline, feel free to leave the keys as they are. You can always replace them with your own keys later!


---

**To configure this:**
1. go to your server on pythonanythwere.com, specify the 
2. In the configuration section, modify the variables `encryptionKey` with your own key. 

Go to WEB tab and hit reload domain thing. otherwise can take a while to be online.

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Decryption/decryption1.png" alt="C# implementation" style="width: 1000%;">
</div>

  
**How to create an encryption key:**
- The key needs to have a lengths of 16, 24 or 32 bytes
- In our case, we specify the key as a string. The `Encrypt()` function then converts this string into a byte array using UTF-8 encoding. As such, any 16/24/32 character string using ASCII characters will result in a valid encryption key. 

Note that ASCII-based AES keys that are hardcoded in the server backend does not correspond the highest possible security standard (i.e., if the server is compromised the key is available). As a general note, no identifiable personal data should be transferred and stored from the unity app. Only deidentified reserch data. If you plan to transfer sensitive data, make sure to work with your university data protection team to ensure that the data transfer is secure enough to comply with insitutional guidelines. 

**How IVs are transferred**:
Randomly generated each time we are encrypting data. Then added to the beginning of the encrypted data. Trnsferred with the data. As said above, the IV is not secret and can be transferred with the data. 

That’s it! You’re ready to encrypt your data.

Here is a detailed information about the encrytpt function:

[add code of encrypt function]


