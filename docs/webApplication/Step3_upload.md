---
title: Step 3 - Configure web application
layout: default
parent: Notebook 2 – Web Application
---

## Step 3: Configure web application

Now that we've deployed a basic web application, the next step is to configure it to provide the required functionality. Specifically, we need the web application to:

- Establish a connection with the Unity experiment.
- Assign a unique subject number to each participant.
- Receive and store data from the experiment.

All of this functionality is provided in the shared bottle_app_.py script. 

Follow the steps below to configure your web application properly.

**1 .Navgate to application files**
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
**3. Upload Supporting Files**
Upload the provided ID.txt file to the mysite/ directory. This file keeps track of assigned subject numbers, allowing the web application to generate unique IDs for each participant.

![](../../assets/images/server5.png)
---
**4. Create a Directory for Data Storage**
To ensure the web application can store incoming experiment data:

- Create a new directory named files/ inside mysite/ (see step 2 highlighted in picture above).

This is where the received data will be saved.

Once complete, the mysite/ directory structure should look like the example above.

---
---

[Continue with next step](https://lkumle.github.io/onlineVRtoolbox_tutorials/docs/webApplication/Step4_fileNames.html){: .btn }

