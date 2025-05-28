---
title: Step 2 - Upload data to web application
layout: default
parent: Notebook 1 – Data transfer in Unity 
---

## Uploading data to the web application

Now that we’ve established a connection with the web server and retrieved a subject number, we’re ready to start the experiment.

Let’s assume a simple experiment structure with two blocks. After each block, we want upload the data we recorded to the web application. To do this, we'll use a PUT request, which is typically used to send data to a server—in our case, the data recorded during each block.

We’ll handle the upload via the `ConnectionHandler.UploadData()` method, which we'll call from within `ExperimentHandler.cs.`


### Storing data locally before uploading

Before we upload anything, we first need to store the data locally on the participant’s device. The upload function expects data to be saved as a .csv file.

**To do this:**  
- We’ll create one CSV file per block of the experiment.
- These files will be stored in Unity’s **persistent data path** —a platform-specific folder where applications downloaded from a game store have permission to write data. It’s typically used for things like save files or settings; in our case, we’ll use it to store experimental data.

An example implementation is included in ExperimentHandler.cs. The snippet below shows how to:

1. Create a new file in the persistent data path.
2. Write data into the file for the current block.

{: .new-title}
> Note on file naming convention
> 
> To avoid overwriting data when multiple participants upload files, we use the following filename format: *onlineVR_{subjectNumber}_{datetime}_B{block}.csv*  
> This ensures each file is uniquely identifiable on the server. Feel free to adjust the naming convention to suit your needs, but make sure it remains unique for each participant and block/upload.

```c#

    // "run" two blocks
    for(int block = 1; block <= 3; block++){

        // ------------------------- //
        // SET UP DATA FILE
        // ------------------------- //  

        // 1. create filename and filepath for data file
        // our file path has this format: 
        // "onlineVR_{subjectNumber}_{date-time}_B{block}.csv"
        fileName = String.Format("onlineVR_{0}_{1}_B{2}", 
                                SubjectNumber, 
                                DateTime.Now.ToString("yyyy-MM-dd_HH-mm"),
                                block.ToString());

        // 2. we want to write to persistentDataPath!
        filePath = Path.Combine(Application.persistentDataPath,  fileName  + ".csv"); 


        // ------------------------- //
        // WRITE DATA 
        // ------------------------- //
        localDataFile = new StreamWriter(filePath); // create file at specified path

        localDataFile.WriteLine("Well done, you have successfully decrypted this data file."); // write into file

    }
```

### Storing data locally before uploading

Once the block is complete and the data file has been written and closed, we trigger the upload by calling:

```c#
    // ------------------------- //
    // UPLOAD DATA TO SERVER
    // ------------------------- //
    localDataFile.Close(); // close file

    ConnectionHandler.UploadData(fileName, filePath); // upload data to server

```
This sends the CSV file to the web application via a PUT request.

---
---

### Detail: `Upload()` function
Let's also explore this function in a bit more detail. This is not necessary if you simply want to use the provided function. In case you are interested or want to build on the provided code, let's have a look at the `Upload()` function in the `ConnectionHandler.cs` file.

Note that we will explore the encryption process in more detail in [Notebook 3](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/encryption/Index.html), but for now, we will assume that the `Encrypt()` function is defined and works as intended.


**Explanation:**
1. Retrieves the file name and reads the stored experiment data from the specified file path on participants device.
2. Encrypts the  data.
3. Combines the file name and encrypted data into a single byte array for uploading.
4. Sends a PUT request to the server with the combined data.
5. If there is a network or HTTP error, the function retries the upload (recursive call).


```c#
    IEnumerator Upload()
    {
        /// ---------------- 
        // Function Name: Upload()
        // Purpose: Uploads the experiment data (including a filename) to the server. The data is encrypted before being uploaded.
        // The function handles both network/HTTP errors and retries the upload if it fails.
        // Parameters: None
        // Return Value: IEnumerator (handles asynchronous requests via Unity's coroutine system)
        //
        // Explanation:
        // 1. Retrieves the file name and reads the stored experiment data from the specified file path on participants device.
        // 2. Encrypts the  data.
        // 3. Combines the file name and encrypted data into a single byte array for uploading.
        // 4. Sends a PUT request to the server with the combined data.
        // 5. If there is a network or HTTP error, the function retries the upload (recursive call).
        /// ---------------- 

        
        // Retrieve the file name and read the file's bytes
        // We want to also upload the file name!
        byte[] filenameBytes = System.Text.Encoding.UTF8.GetBytes(fileName_upload); 
        

        // Read in temporarily stored data and then encrypt 
        byte[] dataBytes = System.IO.File.ReadAllBytes(filePath_upload); // read data from user PC
        byte[] encryptedFileBytes = Encrypt(dataBytes); // encrypt participant data

        // Combine the file name bytes and the encrypted file data into a single byte array
        byte[] uploadData = new byte[filenameBytes.Length + encryptedFileBytes.Length]; 
        System.Buffer.BlockCopy(filenameBytes, 0, uploadData, 0, filenameBytes.Length);
        System.Buffer.BlockCopy(encryptedFileBytes, 0, uploadData, filenameBytes.Length, encryptedFileBytes.Length);
        
        // Create a PUT request with the combined data 
        UnityWebRequest www = UnityWebRequest.Put(serverAddress, uploadData);
        yield return www.SendWebRequest(); // Wait for the upload to complete

         // Handle potential network or HTTP errors
        //if (www.isNetworkError || www.isHttpError)
        if (www.result == UnityWebRequest.Result.ConnectionError || www.result == UnityWebRequest.Result.ProtocolError)
        {
            Debug.Log(www.error);
            StartCoroutine(Upload()); // Retry the upload in case of error
        }
        else
        {
            Debug.Log("Upload complete!"); // Notify that the upload was successful
        }
    }

```




