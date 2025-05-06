---
title: Step 4 - Handling file names
layout: default
parent: Notebook 2 – Web Application
---


## Step 4: Handling filenames 

If you are simply recreating the provided pipeline, you have now successfully set up your web application! 

Let’s quickly discuss how to handle filenames if you need to customize them for your study.

By default, the filename in the provided Unity project is formatted as onlineVR_{subID}_{timestamp}, where {subID} is the assigned subject number and {timestamp} represents the time the data files was creates. 
  
  
To ensure the data is stored with this same filename on PythonAnywhere, we can embed the filename within the encrypted data before uploading it. This effectively makes the filename a "header" in the byte stream.

---
**How the Bottle App Processes Filenames**

When the Bottle app receives the data:
1. It extracts the filename from the incoming data (the filename itself is not encrypted).
2. It removes the filename from the byte stream.
3. It stores the remaining encrypted data in the */files/* directory.

![](../../assets/images/server6.png)

---
**Specifying the Filename Length**

To correctly extract the filename, the Bottle app needs to know its exact length — ensuring it reads only the filename and not any extra data.

To configure this:
1. Open the bottle_app.py script.
2. Locate the configuration section.
3. Adjust the filename_length variable to match the filename size (in bytes).

By setting this correctly, you ensure accurate filename handling while maintaining data integrity.


![](../../assets/images/server7.jpg)
