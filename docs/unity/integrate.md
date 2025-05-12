---
title: Step 3 - Intergrate into existing Unity task
layout: default
parent: Notebook 1 – Data transfer in Unity 
---

## Integrating into existing Unity project

How do you add it to your own unity task? This is the ultimate goal. 

We packages it in a Unity package onlineVR. 

## Install onlineVR package in Unity

- Package manager: install from disk
- select in folder. 


## add prefab to unity project

- go to packages. add ConnectionMenu to unity project. 
- open ConnectionHandler.cs: edit URL to your web application

## add to experiment logic
follow example provided in Experimenthandler. Here we need to trigger the appropriate functions at the correct time

** Testing connection**
This code can be copied at beginning of Experiment procedure. Make sure whatever script handles the experiment flow has access to the ConnectionMeu and ConnectionHandler.cs

[CODE FOR TEST CONNECTION]



**Uploading data**

- at the end of experiment of lock, use upload function to upoad data.

- Note on saving data. In this tutorial, we just uploaded prestored data. because we just wanted to test the data transfer functionality. We are looking for that dat at "persistent data path". You should store your data there as welll once you implement it. 


That"s is! test your set up using 