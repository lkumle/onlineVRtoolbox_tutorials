---
title: Step 3 - Configure web application
layout: default
parent: Notebook 2 – Web Application
---

## Step 3: Configure web application

Now that we've deployed a basic web application, the next step is to configure it to provide the required functionality. Specifically, we need the web application to:

- Establish a connection with the Unity experiment (GET request)
  - Assign a unique subject number to each participant as part of the GET request
- Receive and store data from the Unity task (PUT request)

All of this functionality is provided in the shared bottle_app.py script. 







Follow the steps below to configure your web application properly.
---
---
**1. Navgate to application files**
1. Navigate to the "files" tab. 
2. Within files, navigate to *mysite/*. 

![](../../assets/images/server4.png)

---
**2. Replace the Default Bottle Script**

Inside the mysite/ directory:
- Open the existing bottle_app.py file.
- Replace its content with the code from the provided bottle_app.py script.
- Save and close the file.

---
**3. Upload Supporting Files and Directories**  

1. Upload the provided ID.txt file to the mysite/ directory (using the "Upload a file" button). 
This file keeps track of assigned subject numbers, allowing the web application to generate unique IDs for each participant.
2. Create a new directory named *files/* inside mysite/. 
This is where the web application will store incoming data. 


![](../../assets/images/server5.png)


Once complete, the mysite/ directory structure should look like the example above.

---
---

[Continue with next step](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Step4_fileNames.html){: .btn }

