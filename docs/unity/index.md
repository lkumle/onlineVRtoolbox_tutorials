---
title: Notebook 1 – Data Transfer in Unity  
nav_order: 2  
layout: default  
---

# Notebook 1: Implementing Data Transfer Functionality in Unity

This notebook walks you through how to implement data transfer functionality within a Unity project.

We provide a **template Unity project** with the minimal setup required to communicate with a web application. This notebook explains the key components of that template and shows you how to replicate a basic working pipeline for data exchange between Unity and a server.

We recommend beginning by replicating this minimal pipeline. Once the pipeline is working in the template project, you can follow [Step 3: Integration](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/integrate.html) to add this functionality to your own Unity project.

{: .new-title}
> Get Started  
>
> To begin, open the **"4_unityTemplateTask"** project in the Unity Editor.

---

### Overview of the Template Unity Task

First, we’ll explore the Unity template project and its structure. This project is already set up to demonstrate the basic data transfer workflow, so no additional configuration is needed at this stage.

The two key components in the Unity hierarchy are:

- **ExperimentHandler** GameObject  
  – With the attached `ExperimentHandler.cs` script  
- **ConnectionMenu** Prefab  
  – With the attached `ConnectionHandler.cs` script

![](../../assets/images/unity0.png)

The template also includes a basic scene—an empty room—as a placeholder for your actual experimental environment.

---

### Key Components

**ExperimentHandler.cs**  
This script defines the overall experimental logic and sequence (e.g. blocks, trials). In the template, the sequence includes two placeholder blocks to simulate progression through an experiment. It also calls functions from the `ConnectionHandler.cs` script to manage data transfer such as testing the server connection or uploading data. 

**ConnectionMenu & ConnectionHandler.cs**  
These elements implement the data transfer and are part of the provided onlineVR Unity package.  

- The **ConnectionMenu** provides a simple UI that shows connection status and allows retrying the connection if needed. It also prevents the participant from proceeding until a connection is established.
- The **`ConnectionHandler.cs`** script includes preconfigured functions for:
  - Sending **GET** requests (e.g. to test the connection and retrieve a subject number)
  - Sending **PUT** requests (e.g. to upload data at the end of a block or experiment)

We’ll review these functions and scripts in detail in the following steps.

![](../../assets/images/unity1.png)

[Continue to the next step](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Step2_webapp.html){: .btn }
