---
title: Step 3 - Configure web application
layout: default
parent: Notebook 2 – Web Application
---

# Step 2: Set up web application
{: .no_toc }




## Uplaod files and configure web application

Now that we've deployed a basic web application, the next step is to configure it to provide the required functionality. Specifically, we need the web application to:

- Establish a connection with the Unity experiment.
- Assign a unique subject number to each participant.
- Receive and store data from the experiment.

All of this functionality is provided in the shared bottle_app_template.py script. Follow the steps below to configure your web application properly.

**Step 1: Navgate to application files**
1. Navigate to the "files" tab. 
2. Within files, navigate to *mysite/*. 

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server4.png" alt="Quickstart Bottle" style="width: 1000%;">
</div>

---
**Step 2: Replace the Default Bottle Script**

Inside the mysite/ directory:
- Open the existing bottle_app.py file.
- Replace its content with the code from the provided bottle_app_template.py script.
- Save and close the file.

---
**Step 3: Upload Supporting Files**
Upload the provided ID.txt file to the mysite/ directory. This file keeps track of assigned subject numbers, allowing the web application to generate unique IDs for participants.

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server5.png" alt="Quickstart Bottle" style="width: 1000%;">
</div>

---
**Step 4: Create a Directory for Data Storage**
To ensure the web application can store incoming experiment data:

- Create a new directory named files/ inside mysite/. (see step 2 highlighted in picture above)
- This is where the received data will be saved.

Once complete, the mysite/ directory structure should look like the example above.

---
---

