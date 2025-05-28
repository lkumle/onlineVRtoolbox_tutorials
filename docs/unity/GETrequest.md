---
title: Step 1 - Recieve data from web application
layout: default
parent: Notebook 1 – Data transfer in Unity 
---


## Establishing a connection with the web server and retrieving a subject number

Before participants begin the experiment, we first need to establish a connection with the server. This serves two purposes:

1. Ensure internet connectivity, so we know data uploads will be possible later.
2. Retrieve a unique subject number for the participant from the server (along with an encryption key—see Notebook 3 for details).

To do this, we use a GET request—a standard HTTP request used to retrieve data from a server. In our case, we send a GET request to the web application, and the server responds with a subject number (and encryption key).

### How It Works in the Template Project
We begin the experiment by testing the connection. This is handled in ExperimentHandler.cs and follows this basic logic:

1. **Show the connection menu** to inform the participant (this will automatically disappear once the connection is established)
2. **Call testConnection()** from ConnectionHandler.cs to send the GET request.
3. **Check the result with the connectionCheck()** helper function.
4. If the connection is successful, **retrieve the subject number** using getSubjectNumber() from ConnectionHandler.cs.   

If the connection test fails, the experiment is halted early to avoid running a session without the ability to upload data.

```c#
    // ------------------------------------------------------------------------------------------------ // 
    // Coroutine determining Experiment sequence
    // ------------------------------------------------------------------------------------------------ //
    private IEnumerator ExperimentSequence(){

        // ==================================================== //
        // ------ 1: establish connection with web server ----- //
        // ==================================================== //

        // display connection menu: This notifies participants that the connection is being established
        ConnectionHandler.displayMenu(true); 

        // Start the testConnection coroutine and wait for it to complete
        // --> connects to server and queeries the server for the subject number and encryption key
        yield return StartCoroutine(ConnectionHandler.testConnection());

        // Check if connection was established successfully 
        // -> terminate experiment if connection failed
        bool  connectionStatus = ConnectionHandler.connectionCheck();

        if(!connectionStatus){
           // stop coroutine
            Debug.Log("Connection failed. Stopping experiment.");
            yield break;
        }

        // query assigned subject number from connection handler
        SubjectNumber = ConnectionHandler.getSubjectNumber();
```

---
---

### Detail: `testConnection()` function
In case you are simply trying to use the onlineVR-toolbox or replicating the minimal pipeline, learning how to call the relevant functions within your experimental logic might be all you need to know. However, if you are interested in how the `testConnection()` routine works, let's have a look at it in detail.

The routine starts by initiating a GET request to the specified server address. It then waits for the request to complete. If there's a network or HTTP error (e.g., the server is unreachable), it shows a failure message ("Connection failed. Retrying...") on the ConnectionMenu UI and automatically retries the connection after a short delay (4 seconds). The retry mechanism is limited to a maximum of 3 attempts. If all attempts fail, it displays a message indicating that the connection could not be established ("Connection failed. There may be a problem with our servers, try again later."). At t

If the connection succeeds, it stores the servers response (which we will define in [Notebook 2](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Index.html)), and extracts the subject ID (`queriedID`)and encryption key (`encryptionKey`) from the response,  sets the `connectionOK` flag to true, which indicates that the connection was successful, and finally disables the connection menu UI (as it is no longer needed). This allows the experiment to proceed with the assigned subject number and encryption key.

```c#

    public IEnumerator testConnection(){

        /// ---------------- 
        // Function Name: testConnection()
        // Purpose: Attempts to connect to the external server and checks its reachability. Handles network/HTTP errors and retries if needed.
        // Return Value: IEnumerator (handles asynchronous requests via Unity's coroutine system)
        //
        // Explanation:
        // 1. Sends a GET request to the server at the provided serverAddress.
        // 2. Waits for the request to complete.
        // 3. If there's a network/HTTP error, checks retry count (up to 3 attempts) and shows a failure message with the option to retry.
        // 4. If the connection succeeds, logs the response and sets the subject number (ExperimentHandler.SubjectNumber), then activates the continue button.
        /// ---------------- 

        
        // Initiates a GET request to the specified server address:
        /*  - test the connection to an external server: check if the server is reachable and if it returns a valid response. */
        UnityWebRequest www = UnityWebRequest.Get(serverAddress);
        yield return www.SendWebRequest();

        // IN CASE OF NETWORK ERROR: (network error or an HTTP error during the request)
        if (www.result == UnityWebRequest.Result.ConnectionError || www.result == UnityWebRequest.Result.ProtocolError)
        {
            
            if (retryCount < maxRetry){ // tried connecting less than maxRetry times
                // notify participant that conection was not sucessful
                StatusText.SetText("Connection failed. Retrying...");

                // retry connection
                yield return new WaitForSeconds(4);  //wait 4 seconds before retrying
                StartCoroutine(testConnection());
                retryCount += 1; // keep track of how many times we tried to connect

            }
            else
            {
                // notify participant that conection was not sucessful and experiment cannot be started
                StatusText.SetText("Connection failed. There may be a problem with our servers, try again later.");
            }
        }
         // SUCCESSFUL CONNECTION: 
        else
        {
            // display successful connection message (might disappear quickly because main experiment starts) 
            StatusText.SetText("Connection successful.");

            // get server response (format = {ID}: {encryption key})
            string serverResponse = www.downloadHandler.text;

            // 1. Store assigned ID
            //queriedID = www.downloadHandler.text.Substring(37);
            queriedID = serverResponse.Split(':')[0].Trim(); // ID is first part of server response

            // 2. store recieved encryption key
            encryptionKey = serverResponse.Split(':')[1].Trim(); // encryption key is second part of server response
            
            Debug.Log("Subject ID: " + queriedID);
            Debug.Log("Encryption Key: " + encryptionKey);

            // 3. signal that connection is established
            connectionOK = true; 

            // 4. disable connection menu (we are ready to start the experiment)
            displayMenu(false); 
        }
    }
```

[Continue with Step 2](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/PUTrequest.html){: .btn }