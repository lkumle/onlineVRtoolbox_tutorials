---
title: Notebook 1 – Data transfer in Unity 
nav_order: 2
layout: default
---

# Notebook 1: Implementing data transfer functionality within Unity 

In this notebook, we will walk through how to implement data transfer functionality inside a Unity project. 

To do so, we created a template Unity project with the minimal required functionality. In this Notebook, we will first go through the implementation of the critical functionalities within this template project. We recommend first replicating this minimal pipeline by following Notebook 1 ([Step 1](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/GETrequest.html) and [Step 2](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/PUTrequest.html)) and Notebook 2. 

Ultimately, the goal of this Notebook is to enable you to integrate the provided implementation into your own Unity project. Once you are able to send and receive data from the *template Unity task* to your web application by replicating the minimal pipeline, you can move on to integrating the provided functionality within your own Unity project by following [Step 3](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/integrate.html). 

{: .new-title}
> Get started
> 
> To get started, download and open the "onlineTask_template" Unity project within the Unity Editor.

--- 
--- 

## Overview of template Unity task

First, we will go through the overall pipeline by looking at the Unity template task. This is already set up - so we will just explore. 

Let's familiarise ourselves with the Unity project, starting with the hierarchy inside the Unity Editor. 

In this provided template project, two components are important (you can find both in the hierarchy of the Unity Editor; also see Figure below):
- ExperimentHandler game object (and the attached `ExperimentHandler.cs` script)
- ConnectionMenu Prefab (and the attached `ConnectionHandler.cs` script)

![](../../assets/images/unity0.png)

Additionally, we added a "basic scene" which is just an empty room as a stand-in for the actual task environment. Lastly, the template task contains a "Player" gameObject, which is part of the SteamVR SDK integration.

{: .highlight}
Replicating the pipeline within the template Unity task does not require the full VR setup. If you want to test this without setting up VR, simply deactivate the Player GameObject (recommended). 

---

Let's explore how both the ExperimentHandler (and the attached `ExperimentHandler.cs` script) and the ConnectionMenu Prefab (and the attached `ConnectionHandler.cs` script) work together to implement data transfer functionalities!



**ExperimentHandler**

The `ExperimentHandler.cs` script attached to the ExperimentHandler game object defines the overall experimental logic and sequence. That is, this script handles the sequential logic of defining blocks, trials, etc. Within the template project, we created a minimal experimental sequence of just two empty blocks (nothing is happening besides uploading a preexisting data file) as a stand-in for the actual experimental logic. 

Within this file, we then call the appropriate functions at the required time from the ConnectionHandler.cs script to test the connection and upload data. 

**ConnectionMenu and ConnectionHandler.cs**  

The ConnectionMenu and `ConnectionHandler.cs` are part of the implementation we share in the onlineVR Unity package. This code handles the actual data transfer and is preconfigured.   

In the ConnectionMenu, we have a simple UI that displays the connection status and allows the user to retry connecting if needed. Displaying this UI menu at the start of the experiment additionally ensures that the participant cannot advance to the main experiment until an internet connection is established. 

The attached `ConnectionHandler.cs` contains functions that handle the actual data transfer. This includes sending GET requests to the web application to test the connection and assigning a subject number, as well as sending PUT requests to upload data at specified times (e.g., at the end of the experiment or block). We will go through these functions in detail in the next steps.



![](../../assets/images/unity1.png)


[Continue with next step](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Step2_webapp.html){: .btn }

