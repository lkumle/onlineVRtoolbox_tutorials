---
title: Notebook 1 – Data transfer in Unity 
nav_order: 2
layout: default
---

# Notebook 1: Implementing data transfer functionality within Unity 


In this notebook, we will walk through how to implement data transfer functionality inside the unity project itself. 

For this, we created a template Unity project with the minimal required functionality. In this Notebook, we will go through the implementation of the critical fucntionalities within this template project. 

{: .new-title}
> Get started
> 
> To get started, download and open the "onlineTask_template" Unity project within the Unitor Editor.


The goal of this Notebook is to enable you to ultimately integrate the provided implementation into your own Unity project. 
  


Lastly, we discuss how to integrate this functionality into your own unity projects. Overall, we hope this enables you to quickly solve this major challenge of testing unity projects online. 


{: .important-title}
Here, share project is a VR project using the steamVR SDK. However, replicating the pipeline does not require the full VR setup. If you want to test this without setting up VR, simply deactivate the Player GameObject (recommended). 


--- 
--- 

## Overview

In this provided template project, two scripts are important (you can find both in the Assets folder; alo see Figure below):
- ExperimentHandler.cs (attached to ExperimentHandler gameObject)
- ConnectionHandler.cs (attached to ConnectionMenu PreFab)

We additionally added a "basic scene" which is just a empty room. We will treat this as a stand-in for the actual experiment virtual environment.


![](../../assets/images/unity0.png)


**Experimenthandler.cs** defines the overall experimental logic and sequence. That is, this script handles the sequential logic of defining  blocks, trials etc. 
Here, we created a minimal experimetnal sequence of just two empty blocks (nothing is happening besides creating a small data frame) as a stand in to the actual experimantal logic. We did this to show how the data transfer functionality can and should be integrated into an existing experimental logic. 

ConnectionHandler.cs handles the actual functionality needed to communicate with server (see Fig X highlighted in orange). 

**There are two main functions and stages of this.**
1. Testing connection & assigning subject number once sucessful 
3. Uplaod data at end of each block. 

For all of this, there are two main routines: testconnection() and Upload(). For third main function Encrypt, see Notebook 3. 

Then there are a few helper functions which lets us trigger these routines and embedd tehse in our experiment logic. 

Below, we go into detail how this works. 

![](../../assets/images/unity1.png)


