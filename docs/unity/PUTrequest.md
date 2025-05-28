---
title: Step 2 - Upload data to web application
layout: default
parent: Notebook 1 – Data transfer in Unity 
---

## Uploading data to the web application

Now that we have established a connection with the web server and retrieved a subject number, we can start the experiment. Let's assume we have a simple experiment with two blocks. In each block, we want to collect some data and then upload it to the web application. To upload data to the web application, we need to implement a PUT request. A PUT request is used to send data to the server, in our case, the experiment data collected during the experiment.

Let's first see how we can address the upload functionality in the `ExperimentHandler.cs`, where we can call the `ConnectionHandler.UploadData()` function to trigger the upload process.


First, a note on storing data locally before uploading it: The Upload function looks for locally stored data in csv format. That is, on the participants device. That is, we need to create a file on the participants device where we can store the data collected during the experiment.
To do this, we will create a new file for each block of the experiment. This file will be stored in the **persistent data path** of the application, which is a location where a unity application downloaded from a game store has writing acces. Usually this might be used to e.g., store game progress or settings. In our case, we will use it to store the experiment data collected during the experiment.

We included an example of how to create a file in the persistent data path in the `ExperimentHandler.cs` file. The code snippet below shows how to create a file for each block of the experiment and then write some data into it. A note on filename: to make these unique on the server and avoid writing over existing files, we use the structure "onlineVR_{subjectNumber}_{date-time}_B{block}.csv".

Then, when we are ready to upload that data, we can close it and provide the `ConnectionHandler.UploadData()` function with the filename and filepath of the data file. This then uploads the data to the web application.

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


        // ------------------------- //
        // UPLOAD DATA TO SERVER
        // ------------------------- //
        localDataFile.Close(); // close file

        ConnectionHandler.UploadData(fileName, filePath); // upload data to server


        //wait 5 seconds in between "blocks" (this is just for mock purposes )
        yield return new WaitForSeconds(5);

        Debug.Log("Block " + block + " finished");

    }
```

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




