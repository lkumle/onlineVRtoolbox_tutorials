# 2. Set up web application

Authors:   
last edited:  

citation:

---

In this tutorial, we will walk through the process of setting up a **web application** using **Bottle** on a remote **server** with **PythonAnywhere**. This will allow us to send and receive data between a Unity experiment and a server.

A web application is a program that runs on a server (e.g., PythonAnywhere) and can be accessed through a web browser or another client, such as Unity. Bottle is a lightweight Python web framework that simplifies the creation of web applications with minimal code.

While we could run our Bottle web application on any server, PythonAnywhere streamlines the process by providing:

- A hosted server that runs our Bottle app without requiring a dedicated machine.
- An file system for managing scripts and data.
- Automatic HTTPS support for secure data transmission.
- A web-based interface for easy configuration and monitoring.


<div style="background-color: #E2F0DB; border-left: 6px solid #B5C8A8; padding: 10px;">
  <strong>To get started:</strong> Download the matrials provided at LINK and follow the steps below!
</div>

---
---

### 2.1	Create account on pythonanywhere.com

Navigate to pythonanywhere.com and create a [free beginner account](https://www.pythonanywhere.com/pricing/) which allows to create one *web application* – exactly what we need! 

Simply follow the steps and log-in once you have created an account. The user name you choose will later be your server address (but this will never be visible to anyone exept for you and your collaborators). 

If you are in the EU, you will additionally be prompted to decide whteher you want your account and web applications to be hosted on servers in the EU or the US. According to pythonanywhere, both [comply with GDPR](https://blog.pythonanywhere.com/176/) but choosing EU hosted servers might provide additional reassurance that all parties involved in data transfer (pythonanywhere is a "data processor") are compliant with GDPR laws. 

<div style="background-color: #FDF1D3; border-left: 6px solid #F2AA84; padding: 10px;">
  <strong>Note:</strong> The free beginner account provides 512 MB disk space. Depending on how large the data files are you intend on trasnferring, data should be downloaded and removed from pythonanywhere periodically during data collection. 
</div>

---
---

### 2.2	Create basic web application

Next, we need to create and configure a web application on PythonAnywhere. Fortunately, PythonAnywhere provides a straightforward way to deploy and manage web apps with minimal effort.

**Step 1: Add a new web application**
1. Navigate to the "Web" tab in your PythonAnywhere dashboard.
2. Click "+ Add a new web app" to start the setup process.

![Create web application](/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server1.png)

---
**Step 2: Configure the web application**

If you are using the provided bottle app script, select the following to configure the web application: 
- **python framework:** Bottle

- **python version:** 3.7

Under "Quickstart new Bottle project", do not modify the default path. The path should look like the example below. (*Note:* "TutorialTemplate" is the PythonAnywhere account name used for this tutorial. In your case, it will display your own account name.)

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server2.png" alt="Quickstart Bottle" style="width: 80%;">
</div>

---
**Step 3: Finalizing the Setup**

Once the setup is complete, you will be redirected to a web app configuration page similar to the one below.

- At the top, you will see your web address.
- You will need to use this exact URL in your experiment code (see Step 1: Configure Unity Code).

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server3.png" alt="Quickstart Bottle" style="width: 1000%;">
</div>
 

<div style="background-color: #FDF1D3; border-left: 6px solid #F2AA84; padding: 10px;">
  <strong>Note:</strong> The web application will run for 3 months and then is automatically disabled. You will recieve an reminder email before the application is disabled and can renew it at any time. 
</div>


---
---

### 2.3	Uplaod files and configure web application

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

### 2.4 Optional: Handling filenames in bottle_app.py

If you are simply recreating the provided pipeline, you have now successfully set up your web application! 

Let’s quicklydiscuss how to handle filenames if you need to customize them for your study.

By default, the filename in the provided Unity project is formatted as onlineVR_{subID}_{timestamp}, where {subID} is the assigned subject number and {timestamp} represents the recording time. 
To ensure the data is stored with this same filename on PythonAnywhere, we embed the filename within the encrypted data before uploading it. This effectively makes the filename a "header" in the byte stream.

---
**How the Bottle App Processes Filenames**

When the Bottle app receives the data:
1. It extracts the filename from the incoming data (the filename itself is not encrypted).
2. It removes the filename from the byte stream.
3. It stores the remaining encrypted data in the */files/* directory.

<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server6.png" alt="Quickstart Bottle" style="width: 80%;">
</div>

---
**Specifying the Filename Length**

To correctly extract the filename, the Bottle app needs to know its exact length — ensuring it reads only the filename and not any extra data.

To configure this:
1. Open the bottle_app.py script.
2. Locate the configuration section.
3. Adjust the filename_length variable to match the filename size (in bytes).

By setting this correctly, you ensure accurate filename handling while maintaining data integrity.


<div style="text-align: center;">
  <img src="/Users/levikumle/Library/CloudStorage/OneDrive-Nexus365/1_Research/ObjectSorting/onlineVR/5_tutorialResources/Server/server7.png" alt="Quickstart Bottle" style="width: 100%;">
</div>
