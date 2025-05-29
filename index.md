---
title: Overview and Quickstart  
layout: home  
nav_order: 1  
---

# Running Virtual Reality Experiments Online: A Brief Introduction and Tutorial

**Authors:** Levi Kumle, Alfie Brazier, Joel Kovoor, Johannes Keil, Anna C. Nobre & Dejan Draschkow

## Overview

This website hosts step-by-step **practical notebooks** accompanying the tutorial *Running Virtual Reality Experiments Online: A Brief Introduction and Tutorial* and the [onlineVR-toolbox](https://github.com/lkumle/onlineVR-toolbox).

The goal of the onlineVR-toolbox and these notebooks is to address a core challenge in online VR data collection: implementing data transfer functionality.

We recommend starting with the main tutorial paper for a high-level overview of the unique challenges of conducting VR experiments online.

{: .important-title}
> Overview of provided resources
>
> These materials present a minimal working pipeline that establishes communication between a Unity-based task and a web application.

The resources consist of three notebooks, each covering a distinct part of the pipeline:

**Notebook 1:** Implementing data transfer functionality within Unity  
**Notebook 2:** Setting up a web application that the Unity task can access  
**Notebook 3:** Ensuring data security through encryption and decryption  

Together with the provided Unity project and scripts in the onlineVR-toolbox, these notebooks guide you through replicating a complete pipeline that sends (encrypted) data from a Unity task to a web server, where it can be downloaded and decrypted.

## Requirements

{: .highlight}
This is a fully self-contained tutorial—you can replicate the minimal working pipeline using only the materials provided.

You don’t need an existing Unity project or a published build (e.g., on Steam) to follow along. Everything you need is included.

We recommend working through the notebooks and replicating the example pipeline before integrating it into your own VR task. This approach makes troubleshooting easier and gives you an understanding of the necessary components before diving into your own (more complex) use case.

**Technical requirements:**

To replicate the pipeline locally, you’ll need:

- Unity (Editor version: **2022.3.59f1**)  
- Jupyter Notebooks (or any Python environment)

This tutorial assumes basic familiarity with Unity, C#, and Python. However, **no additional coding is required** to replicate the core functionality—we provide pre-implemented code for all steps and guide you through how to configure it.

## Getting Started

To begin, download the [onlineVR-toolbox](https://github.com/lkumle/onlineVR-toolbox).

[Start with Notebook 1](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/unity/){: .btn }

---

{: .new-title }
> Feedback and Contributions  
>
> This is an evolving resource created to support researchers who want to run VR experiments online. We welcome feedback, suggestions, and contributions.
>
> If you encounter issues or develop improvements not covered in these materials, we’d love to hear from you so we can grow this resource with the growing experience of the community.

---

## Get in Touch

Levi Kumle – levi.kumle@psy.ox.ac.uk  
Dejan Draschkow – dejan.draschkow@psy.ox.ac.uk  
**Adaptive Behaviour & Cognition Lab** – abclab@psy.ox.ac.uk  
[https://www.psy.ox.ac.uk/research/adaptive-behaviour-cognition](https://www.psy.ox.ac.uk/research/adaptive-behaviour-cognition)

---

![](assets/images/logo.png)
