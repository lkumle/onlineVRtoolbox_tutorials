---
title: Unity: Implement data transfer
nav_order: 2
---

# 1. Implementing data transfer functionality in Unity project

Authors:   
last edited:  

citation:

---

In this notebook, we will walk through how to implement data transfer functionality inside the unity project itself . We will assume that you know unity well enough to implement your experiments in unity. 

For this, we created a template with the minimal required functionality. That is, connect to the server, get a subject number. Whem prompoted, upload data to the server. 

In this notebook, we will go through the implementation of this minimal template. In Notebook 2, you will then create theweb aplication itslef that recieves tha data. 

In the end, you will have a minimal working version of a unity project and a web server with remote data transfer functionality.  

Lastly, we discuss how to integrate this functionality into your own unity projects. Overall, we hope this enables you to quickly solve this major challenge of testing unity projects online. 


<div style="background-color: #E2F0DB; border-left: 6px solid #B5C8A8; padding: 10px;">
  <strong>To get started:</strong> Download the matrials provided at LINK and follow the steps below!
</div>

--- 
---

**A note on technical requirements:**

- versions we used. 


<div style="background-color: #FDF1D3; border-left: 6px solid #F2AA84; padding: 10px;">
  <strong>Note:</strong> he shared project is a VR project using the steamVR SDK. However, replicating the pipeline does not require the full VR setup.
  - If you want to test this without setting up VR, simply deactivate the Player GameObject.  (see below)
</div>

--- 
--- 

### 1.1. Overview

In this provided template project, two scripts are important (you can find both in the Assets folder; alo see Figure below):
- ExperimentHandler.cs (attached to ExperimentHandler gameObject)
- ConnectionHandler.cs (attached to ConnectionMenu PreFab)

We additionally added a "basic scene" which is just a empty room. We will treat this as a stand-in for the actual experiment virtual environment.


<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Unity_template/unity0.png" alt="Overview Unity" style="width: 60%;">
</div>


**Experimenthandler.cs** defines the overall experimental logic and sequence. That is, this script handles the sequential logic of defining  blocks, trials etc. 
Here, we created a minimal experimetnal sequence of just two empty blocks (nothing is happening besides creating a small data frame) as a stand in to the actual experimantal logic. We did this to show how the data transfer functionality can and should be integrated into an existing experimental logic. 

ConnectionHandler.cs handles the actual functionality needed to communicate with server (see Fig X highlighted in orange). 

**There are two main functions and stages of this.**
1. Testing connection & assigning subject number once sucessful 
3. Uplaod data at end of each block. 

For all of this, there are two main routines: testconnection() and Upload(). For third main function Encrypt, see Notebook 3. 

Then there are a few helper functions which lets us trigger these routines and embedd tehse in our experiment logic. 

Below, we go into detail how this works. 


<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Unity_template/unity1.png" alt="Overview Unity" style="width: 100%;">
</div>


---
---

### 1.2. Establishing connection and assigning subject ID

Before participants start the experiment, we want to make sure that a connection with the server is established. This is important to both make sure that the participant has a working internet coonnection so that data can be uploaded once required. Additionally, we want to assign a uniwue subject ID (add why based on main manuscript stuff: link data accross plattforms, distingiguis general public from research participants, ...). 


To implement this, we start the experiment logic with testing the connection. That is, we display the connectionMenu and trigger the testConnection()-function. 


<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Unity_template/unity2.png" alt="Overview Unity" style="width: 100%;">
</div>


Let's have a look at what the testConnection() function is doing. Feel free to skip this part in case you are simply trying to recreate the pipeline. 

1. Sends a GET request to the server at the provided serverAddress.
- what is a get request? Also see Notebook 2. 
2. Waits for the request to complete.
3. If there's a network/HTTP error, checks retry count (up to 3 attempts) and shows a failure message with the option to retry.
4. If the connection succeeds, logs the response and sets the subjecct number, then removes the connection menu (not needed anymore)

- Status.Text is text object on connectionMenu

```c#
 public IEnumerator testConnection(){
    
        // Initiates a GET request to the specified server address:
        //  - test the connection to an external server: 
        //check if the server is reachable and if it returns a valid response. 
        UnityWebRequest www = UnityWebRequest.Get(serverAddress);
        yield return www.SendWebRequest();

        // IN CASE OF NETWORK ERROR: (network error or an HTTP error during the request)
        if (www.isNetworkError || www.isHttpError)
        {
            // notify participant that conection was not established
            if (retryCount < maxRetry){ // tried connecting less than maxRetry times
                StatusText.SetText("Connection failed. Retrying...");

                // retry connection
                yield return new WaitForSeconds(4);  //wait 4 seconds before retrying
                StartCoroutine(testConnection());
                retryCount += 1;
            }
            else // reached maximal number of retries
            {
                StatusText.SetText("Connection failed. There may be a problem with our servers, try again later.");
            }
        }
         // SUCCESSFUL CONNECTION: -----------
        else
        {
            // 1. Store assigned ID
            // TODO: check what the 37 means??
            queriedID = www.downloadHandler.text.Substring(37);
            
            // 2. signal that connection is established
            connectionOK = true; 

            // 3. disable connection menu
            displayMenu(false); 
        }
    }

```
---
---


### 1.3. Uploading data 

At the end of each block (or experiment but we recommen block), we need to upload our data. 

For this, we need the other main functionality, which is the Upload Coroutine. 

First, a note on storing data locally before uploading it: The Upload function looks for locally stored data. For that, we need to specify the filename and filepath. For each block, we therefore create a new file. 

A note on filename: to make these unique on the server and avoid writing over existing files, we use the structure:

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Unity_template/unity5.png" alt="Overview Unity" style="width: 100%;">
</div>

 
Then, we can trigger the upload routine using the UploadData() helper function. This helper function provides the filename and filepath to the connectionhandler and trigegrs the appropriaze coroutine. 
 

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Unity_template/unity3.png" alt="Overview Unity" style="width: 100%;">
</div>


The upload function in detail does the following: Skip if not interested. 

**Explanation:**
1. Retrieves the file name and reads the stored experiment data from the specified file path on participants device.
2. Encrypts the  data.
3. Combines the file name and encrypted data into a single byte array for uploading.
4. Sends a PUT request to the server with the combined data.
5. If there is a network or HTTP error, the function retries the upload (recursive call).


```c#
 IEnumerator Upload()
    {
        // Get filename in bytes
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
        if (www.isNetworkError || www.isHttpError)
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

### 1.3. Finalise and Test Set up

<div style="background-color: #FDF1D3; border-left: 6px solid #F2AA84; padding: 10px;">
  <strong>Note:</strong> For this to work you already need a working web application. First complete Notebook 2, then come back and configure your unity template. Once you have set up the web application, return here to test the set up!
</div>


- 1. Configure unity with your web address!

--- image here

- 2. Run project
- if connection is successful, you probably don#t even see the connectionMenu. But In unty you should see the following console messages:**

- also check that data files arrived

- Optional: decrypt files to make sure that all is going well!! (move on to Notebook 3 to learn how to do this!)


- 3. Double check that "retrying connection"/ "no connection" works: change the server address to something that is wrong. You should now see the following on the connection Menu

- **messages** 

### 1.4. Add your experiment code

- Note on saving data. In this tutorial, we just uploaded prestored data. because we just wanted to test the data transfer functionality. We are looking for that dat at "persistent data path". You should store your data there as welll once you implement it. 

- add connectionmenu prefab to code
- trigger connection functions at right time (see above)



