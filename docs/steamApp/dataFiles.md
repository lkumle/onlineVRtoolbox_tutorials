---
title: Step 1 - Creating a Unity build
layout: default
parent: Notebook 4 – Creating and uploading a Unity application
---

# Creating a Unity build

In this notebook, we discuss considerations and steps for turning a Unity experiment into an executable file that can be uploaded to Steam. First, we cover handling data paths and data writing, as these likely represent common challenges researchers might face. After that, we summarise resources on how to create the actual build.

## Data Paths and File Access

In many task implementations, it is common to read from or save data to specific file paths. For example, we might load stimuli (e.g., images) or trial files (e.g., specifying conditions and counterbalancing) at the beginning of the experiment or each trial. While running a task within the Unity Editor, we can easily access these files by specifying paths to the project’s *Assets* folder, since we have direct access to the file system.

However, when building the project, the files are compressed and no longer accessible as individual files. Consequently, the *Assets* (or *Resource* folder within *Assets*) folder no longer represents a direct file path. Similarly, when saving data, we cannot specify an absolute file path as a storage location, as we do not know the remote participant’s directory structure, and the build will not have permission to freely read or write to their file system. To address this, we need to adjust how we handle data paths.

### Loading files

Files that need to be accessible at runtime can be placed in the **StreamingAssets** folder, which can be accessed with `Application.streamingAssetsPath`. Any files stored in this folder will be available within the build. The StreamingAssets folder must be located in the root of the Assets directory at: `Assets/StreamingAssets`.

For instance, suppose we want to read a text file that specifies the (randomised) order of conditions for specific subject numbers. 

```c#
// Read in files
Conditions = new StreamReader(
    Path.Combine(Application.streamingAssetsPath, 
    "ConditionFiles/SUB" + SubjectNumber + ".txt")
);
```

For further information, also see [Unity Documentation: Include additional files in a build](https://docs.unity3d.com/6000.2/Documentation/Manual/StreamingAssets.html). 


#### Writing files
 
For saving data to file, Unity provides “Application.persistentDataPath” (also see Notebook 1 for an example), which points to a system-specific location where the build is allowed to store files. In typical game development, this path might be used to save (and later load) progress within a game. When conducting remote VR studies, we can use it to write any experiment data to file, which we can later upload to our server. 

For saving data, Unity provides "Application.persistentDataPath" (also see Notebook 1 for an example), which points to a system-specific location where the build is allowed to store files. In typical game development, this path might be used to save (and later load) game progress. For remote VR studies, we can use it to write experiment data to a file, which can later be uploaded to our server.

```c#

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

      localDataFile.WriteLine("Whatever data is being saved"); // write into file

```


## Creating the build

Finally, we need to turn our experiment into an executable file. We recommend following the official Unity documentation to do so: 

- Tutorial on Unity Learn: [Building the Game](https://learn.unity.com/course/roll-a-ball/tutorial/building-the-game?version=6.0)
- Unity Documentation: [Building and publishing](https://docs.unity3d.com/6000.2/Documentation/Manual/building-and-publishing.html)


Importantly, when testing a build version on the same system as the Unity project, errors might be concealed if the build includes absolute file paths that point to the original, unpackaged Assets folder. To ensure the build functions independently, it is critical to test it in an environment where the original project is not available.



[Continue with Step 2](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/steamApp/buildUpload.html){: .btn }
