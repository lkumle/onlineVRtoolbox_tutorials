---
title: Notebook 3 – Encryption and Decryption 
nav_order: 4
layout: default
---

# Notebook 3: Encryption and Decryption of Data

In this Notebook, we will cover how to ensure data privacy during transmission and storage by using encryption.


## Overview

**Encryption** is a technique used to convert readable information (plaintext) into an unreadable format (ciphertext) to protect it from unauthorized access. In this process, data is scrambled using a secret key. Without this key, the data appears as random noise, making it impossible to interpret. This ensures that only authorized individuals (e.g., researchers with the correct key) can access the original data.

**Decryption** is the reverse process, where ciphertext is transformed back into its original form using the same key used for encryption.

---
---

There are different encryption algorithms. In the resurces provided in the onlineVR-toolbox, we will be using **AES (Advanced Encryption Standard)**, a widely adopted symmetric encryption algorithm. Symmetric means that the same secret key is used for both encryption and decryption. Thanks to its popularity, implementations of AES are available in various programming frameworks, which makes it easy to use. 

To use AES encryption correctly, we need:
- **Encryption Key**: A secret key used for both encryption and decryption. The security of the encryption relies on this key.
- **Initialization Vector (IV)**: A random value used to ensure that the same data encrypted multiple times will produce different ciphertexts each time, adding an extra layer of security. The IV is not secret and will therefore be transferred together with the experiment data. 

---
---


We will first explore how the data is encrypted within Unity. Then, we'll go over the decryption process once the encrypted data is downloaded from the server.

