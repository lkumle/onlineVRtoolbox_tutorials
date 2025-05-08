---
title: Overview and Quickstart
layout: home
nav_order: 1
---

# Running Virtual Reality Experiments Online: A Brief Introduction and Tutorial

**Authors:** Levi Kumle, Alfie Brazier, Joel Kovoor, Johannes Keil, Anna C. Nobre & Dejan Draschkow

## Overview
This website hosts step-by-step **practical Notebooks** accompanying the tutorial *Running Virtual Reality Experiments Online: A Brief Introduction and Tutorial*  and the [onlineVR-toolbox](https://github.com/lkumle/onlineVR-toolbox). 

The aim of these Notebooks and the onlineVR-toolbox is to provide a preimplemented solution for a core challenge of online data collection with VR: Implementing data transfer functionalities.  
We recommend starting with reading the main tutorial paper for a high-level overview of the challenges of collecting data online for VR experiments. 

{: .important}
Within these accompanying materials, we present a minimal pipeline that implements communication between a Unity task and a web application, enabling the Unity task to send and retrieve data through the internet. 

The mterials consist of 3 Notebooks, each tackling a discrete part of the pipline: 
1. **Notebook 1:** Implementing data transfer functionalities within Unity
2. **Notebook 2:** Setting up a web application that the Unity task can access
3. **Notebook 3:** Using decryption to ensure data security

These Noteboks go hand-in-hand with the resources provided within the onlineVR-toolbox. That is, together, they are designed to replicate a minimal pipline that sends (encrypted) data from a unity task to a web server where we can download it and decrypt it. 

## Requirements
{: .highight }
This is a self-contained tutorial: you can replicate the minial pipeline using only the provided materials. 
  
You don't need your own Unity task to be ready. The task also does not need to be uplaoded on Steam (ar another gamestore) or packaged as a build. 
We recommend to first replicate the pipleine provided here. This will make troubleshooting easier and allows you to get a sense of the involved steps. Once you have implemented a working pipeline, you can integrate the provided code and resources into your own Unity task. 

We assume a basic understanding of Unity,  C#,  Python. However, replicating the minimal pipeline does not requrire you to do any coding yourself. 
We provide preimplemented code for all steps, and we go through the steps of how to configure the provided code. 

**Technical requirements:**
Replicating the minimal pipeline on your own machine requires you to have a running version of: 
- Unity (Editor version 2022.3.59f1)
- Jupyter Notebooks (or another Python environment)

## Getting started

To get started, download the [onlineVR-toolbox](https://github.com/lkumle/onlineVR-toolbox). 

Then, start with Notebook 1.

---
---

{: .new-title }
> Feedback and Contributions
>
> This is a developing resource built with the intention to help other researchers to get their VR tasks online. We wellcome contributions, suggestions and feedback to this work. 
>
> If you use this resource and encounter challenges, problems or solutions that are not covered within the materials provided here, we look forward to hearing from you so this resource can grow with the experience of our research community. 

---
--- 


## Get in touch 
Levi Kumle - levi.kumel@psy.ox.ac.uk  
  
Dejan Draschkow - dejan.draschkow@psy.ox.ac.uk
  
AB&C Lab - abclab@psy.ox.ac.uk

![](../../assets/images/logo.png)



