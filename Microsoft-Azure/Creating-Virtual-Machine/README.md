# Overview:

In this lab we will set up an instance of Windows Server 2019 on the Microsoft Azure Cloud, though the process is the same for any type of machine you want to create, more or less. The Azure Cloud is especially useuful for owners of Apple's ARM chips like myself who's primary computers aren't able to run Virtual Box and therefor can't virtualize certain architecture. Microsoft generously provides us with a 30 day, 200 hour, $200 free credit window (whichever comes first). You will want to be sure to shut down your virtual machines when not in use so you don't eat into these free credits, or worse, rack up charges!

### Step One:

The first thing we are going to want to do after logging into portal.azure.com with our Microsoft account is click the Start button under "Start with an Azure free trial".

<img width="1440" alt="Screenshot 2024-05-29 at 1 42 57 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/3303b82e-1f82-4b99-b68c-71c45b908e6f">

We will then be navigated to this page, where we will select "Start Free".

<img width="1440" alt="Screenshot 2024-05-29 at 1 44 11 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/969af5cf-4778-44b7-9a44-308d7bb4874f">

From here we will be prompted to enter our payment details to continue with the process. I've left the payment screens out of this document for obvious reasons, but we will continue from there.

### Step Two:

Next we will be navigated to the "Quickstart Center" which is beyond the scope of this lab, but I would highly reccomend going through these resources to familiarize yourself with the Azure platform.

<img width="1440" alt="Screenshot 2024-05-29 at 1 50 20 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/a43dd67e-f3e9-4d2c-8a89-747456b72ab0">

### Step Three:

Once you are back on the main Azure portal page, go ahead and click the drop down on the top left of page and click "Create a resource".

<img width="1440" alt="Screenshot 2024-05-29 at 2 28 58 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/a8123a60-15c5-4747-8df5-26dbf81debaf">

### Step Four:

From here we will select "Virtual Machine".

<img width="1440" alt="Screenshot 2024-05-29 at 2 30 50 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/ea718603-25cb-43be-8278-079b3e752fe1">

### Step Five:

We will be brought to a page that allows you to configure various settings. The "Basics" tab is where we will configure all the important information.

1) Select your Azure subscription. This will likely be "Azure subcription 1".
2) Create a new Resource Group. I've named mine rg-HomeLabs.
3) Give your virtual machine a name. I've named mine "ADBox" as I will be using it to play around with Active Directory.
4) Select your region.
5) Select your availability options. For my home lab purposes I've chosen "No infrastructure redundancy required".
6) Select your security type. For my purposes "Standard" is adequate.
7) Select the image you'd like to deploy. I've chosen Windows Server 2019.

<img width="1440" alt="Screenshot 2024-05-29 at 2 47 09 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/b7cc9213-088e-4d2a-a686-118143b3c330">

8) Next let's select the size of the Virtual Machine we'd like to create. This is the amount of resources Azure will use for the instance you are creating and will be directly related to how much you will be billed monthly.

<img width="1440" alt="Screenshot 2024-05-29 at 2 47 22 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/71aa1243-060d-4e23-8dbc-515648e59f74">

9) Give your Virtual Machine a Username.
10) Give your Virtual Machine a Password.
11) Make sure you have RDP selected for inbound ports so we can access the machine.
12) Then go ahead and click "Review + create"!

<img width="1440" alt="Screenshot 2024-05-29 at 2 47 25 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/a3e44c8e-71df-44a2-b69c-bf6ad54a855c">

### Step Six:

Assuming your configuration has passed validation, your machine will now be ready to create! You may run across some validation errors, and in that case you will want to go back to the "Basic" tab and make sure the image you've selected doesn't exceed the limitations of your free trial. You can see that running this machine will cost $.2090/USD to run per hour, but initially this cost will be taken from your free trial period credits.

<img width="1440" alt="Screenshot 2024-05-29 at 2 48 40 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f1cca051-139a-4f60-99e7-74a3a2d2176c">

### Step Seven:

Once you've completed step six and clicked "Create", you will be redirected to this page where your deployment will initialize. This will take some time, so hang tight until the pop-up in the top right corner appears and click "Go to resource".

<img width="1440" alt="Screenshot 2024-05-29 at 2 50 42 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/ca34cfc4-f35a-4dbf-a46f-3324544b37b1">

### Step Eight:

On this page you will be shown an overview of your machine. There is a lot to inspect here, so feel free to flip through the different tabs, but for our purposes we will be selecting the "Connect" button.

<img width="1440" alt="Screenshot 2024-05-29 at 2 53 34 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/2eba91dd-c08f-4061-91e1-9cc728610b89">

### Step Nine: 

On this page we will want to click the "Download RDP file" button which will be downloaded straight to your default download path. As long as you have an RDP client installed on your device, you will be brought straight into the login prompt where you enter the password you configured earlier in Step Five. There are native Windows RDP clients available for Windows and macOS, but third party tools exist if you prefer Linux.

<img width="1440" alt="Screenshot 2024-05-29 at 3 27 18 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/017038e6-a885-4707-b016-ee464327a100">

### Step Ten:

And we're in! We now have an instance of Windows Server 2019 running on Azure ready for our Home Labs! You will want to make sure you always close down and turn of your machine via your browser so you don't burn through your free Azure credits, or worse, rack up additional charges once your trial period has expired. Enjoy!

<img width="1440" alt="Screenshot 2024-05-29 at 2 54 47 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/5ef28fd4-22ae-4b9f-ba94-c9e288f99759">
