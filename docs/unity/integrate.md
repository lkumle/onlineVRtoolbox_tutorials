---
title: Step 3 - Intergrate into existing Unity task
layout: default
parent: Notebook 1 – Data Transfer in Unity  
---

## Integrating onlineVR-toolbox into your Unity task

Let's explore how you can integrate this functionality into your own Unity task. 


### 1. Install onlineVR package 

1. Download the onlineVR-toolbox package from the [onlineVR-toolbox GitHub repository](https://github.com/lkumle/onlineVR-toolbox).
2. Open your Unity project.
3. Go to `Package Manager` > `+` > `Add package from disk`.
4. Navigate to the downloaded package folder (`onlineVR-toolbox/1_unityPackage/onlineVR`) and select the `package.json` file within the `onlineVR` folder.

![](../../assets/images/install1.png)

---
### 2. Add ConnectionMenu Prefab

1. In your Unity project, open the scene where you want to integrate the onlineVR-toolbox functionality.
2. In the `Project` window, navigate to the `onlineVR` package folder (path in Editor: `Packages/onlineVR`).
3. Open the `/Editor` folder and locate the `ConnectionMenu` prefab. This prefab contains the UI elements for the connection menu and is already set up with the necessary scripts.
4. Drag and drop the `ConnectionMenu` prefab into your scene hierarchy.

---
### 3. Integrate into existing experiment logic

To fully integrate the onlineVR-toolbox functionality into your existing Unity task, you need to ensure that the dedicated functions and routines from the `ConnectionHandler.cs` script is called at the appropriate times in your experiment logic. 

This typically involves two main steps: establishing a connection at the start of the experiment and uploading data at the end of the experiment or block. You can use the code provided below or explore the `ExperimentHandler.cs` script and Step 1 and 2 of this notebook

**Establishing connection and retrieving a subject number**:
```c#
    // important: create access to ConnectionHandler
    public ConectionHandler ConectionHandler; 

    // display connection menu: This notifies participants that the connection is being established
    ConnectionHandler.displayMenu(true); 

    // Start the testConnection coroutine and wait for it to complete
    yield return StartCoroutine(ConnectionHandler.testConnection());

    // Check if connection was established successfully 
    bool  connectionStatus = ConnectionHandler.connectionCheck();

    if(!connectionStatus){
        // stop coroutine
        Debug.Log("Connection failed. Stopping experiment.");
        yield break;
    }

    // query assigned subject number from connection handler
    SubjectNumber = ConnectionHandler.getSubjectNumber();
```

**Uploading data**:
```c#
    // create filename and filepath for data file
    fileName = String.Format("onlineVR_{0}_{1}_B{2}", 
                            SubjectNumber, 
                            DateTime.Now.ToString("yyyy-MM-dd_HH-mm"),
                            block.ToString());
    filePath = Path.Combine(Application.persistentDataPath,  fileName  + ".csv"); 

    // write data to file (this is a placeholder, you would replace this with your actual data writing logic)
    ConnectionHandler.writeDataToFile(filePath, data);

    // upload data to server
    yield return StartCoroutine(ConnectionHandler.UploadData(filePath));
```

---
### 4. Configure with your web application
To configure the onlineVR package to work with your web application, you need to set the server address in the `ConnectionHandler.cs` script. We go through this in detail in [Notebook 2](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Index.html), where we show how to set up the web application and configure the server address in the Unity project.


### 5. Some additional notes and tips:

**Invariant Culture Setting**  

We recommend setting the current culture to invariant culture in your Unity project. This ensures that data formatting is consistent across different systems and avoids issues with locale-specific formats (e.g., decimal points, date formats). You can do this by adding the following line of code at the start of your main script (also see `ExperimentHandler.cs` in the template project):

```c#
using System.Globalization;

// make sure that culture is set to invariant (for csv writing)
CultureInfo.CurrentCulture = CultureInfo.InvariantCulture;
```

