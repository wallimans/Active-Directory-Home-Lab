# Overview

We will be using the following tools:

For this project we are going to spin up an instance of Windows Server 2019 on Azure, configure it as our Domain Controller, then set it up to run Active Directory. This will require creating an IP addressing scheme, two Virtual NICs (one for the interior and exterior networks), as well as configuring RAS/NAT and DHCP to let any host machines communicate with the outside network and automatically obtain their internal IP addresses. Before we deploy a host machine we will run a Powershell script to automatically provision 1,000 users for our practice purposes.

- Microsoft Azure
- RDP Client
- Windows Server 2019
- Active Directory Services
- Powershell

This file will server as the master document containing all the parts and steps, but you can navigate to individual sections as well:

- [Part One](https://github.com/wallimans/Home-Lab/tree/main/Active-Directory/Part-One)
- Part Two
- Part Three

## Part One: Configuring Windows Server 2019 Ethernet Settings

### Step One: Installing Windows Server 2019 on Azure

Our first step is creating the virtual machine that we will configure as our domain controller and use for the lab. For our purposes we will be using Microsoft's Azure platform to spin up our virtual machines in the cloud. This can be done other ways like running hypervisors such as Virtual Box directly on your machine, but for people with Apple Silicone processors (M1, M2, M3, etc.) you won't be able to run any instances of Windows Server as they aren't supported by ARM processors yet. This way you can follow along regardless of the computer you are using, as long as you can run an RDP client to connect to the boxes, which are available for all major operating systems. I've broken this step down [here](https://github.com/wallimans/Home-Lab/tree/main/Virtual-Machines/Microsoft-Azure/Creating-Windows-Server-2019).

### Step Two: Establishing Ethernet Settings

After downloading the RDP file and logging into the system remotely, we will be greeted by this page:

<img width="1440" alt="Screenshot 2024-06-03 at 2 33 24 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/55c3c953-0ef7-42e6-903c-62f34bf44cc5">

Before we configure anything on the server, let's first set up our IP addressing. To reach to the IP address setings, let's click on the Network icon on the bottom right of the screen and then click "Network".

<img width="1440" alt="Screenshot 2024-06-03 at 2 39 23 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/081b8eba-8b8b-4267-81a0-d8011cd469d1">

From here we will click "Change adapter options":

<img width="1440" alt="Screenshot 2024-06-03 at 2 40 12 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/e2efac9f-1d16-47ef-ba2f-5b6fc4e61223">

And here we can see a single ethernet Network adapter, which we will right click to rename to "_INTERNET_" to make things easier for us down the road when identifying interfaces.

<img width="1440" alt="Screenshot 2024-06-03 at 2 47 11 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/6c6ff135-30c8-4fc5-b38c-0bdd4bf68e83">

In order to create an additional interface for our internal network, we will have to go back to our Azure portal, locate our VM, shut it down, locate it's Network Settings, then click "Attach network interface":

<img width="1440" alt="Screenshot 2024-06-03 at 2 54 53 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/e25acd80-d2b9-4e5b-a3a8-1ef330783ec8">

From here we will be prompted to configure the new Network Interface. We will: 

1) Allocate it to our Azure Resource Group
2) Name the interface
3) Assign it's subnet

<img width="1440" alt="Screenshot 2024-06-03 at 3 05 08 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/44f688aa-da89-4091-a5c3-e62f6155c931">

4) Select it's inbound ports
5) Give it a network address (or choose dynamic)

<img width="1440" alt="Screenshot 2024-06-03 at 3 05 13 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/b59c4cd3-9058-4a1e-a1b0-d42d75de1131">

Loading back into our VM and navigating to the Ethernet options as we did before, we should now be able to see the additional interface that we will right click to rename "_INTERNAL_".

<img width="1440" alt="Screenshot 2024-06-03 at 3 10 46 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/6ab63bd4-c1c6-4f46-ad15-b4358e312e4a">

From here, let's right click our new _INTERNAL_ adapter and navigate to properties, then double click "Internet Protocol Version 4 (TCP/IPv4)". From here, let's click "Use the following IP address" and assign this adapter it's IP address scheme. We are using a 192.168.10.0/24 scheme, so let's give it an address of 192.168.10.1, with a subnet mask of 255.255.255.0, and we can leave the default gateway blank as this server will use itself. Clicking "Use the following DNS server addresses:" we can type 127.0.0.1 (our loopback address) to indicate that the server will use itself for DNS, which comes about when we install Active Directory services. Go ahead and click okay to get out of these prompts.

<img width="1440" alt="Screenshot 2024-06-03 at 3 15 59 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/d2ca3a70-7327-47d4-a6ab-eff12326a7c8">

And that concludes Part One! We've ensured we have both the interfaces we need to continue the project and are ready to move onto Part Two.

## Part Two: Installing Active Directory Services and Creating Admin Account

### Step One: Installing Active Directory

Let's start by navigating back to our Server Manager Dashboard where we will locate the "Add roles and features" button:

<img width="1440" alt="Screenshot 2024-06-04 at 2 16 21 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/525b6b7a-7384-445f-8e09-84a7bf23121d">

Click next until we reach the "Server Selection" tab and select your domain, the click Next:

<img width="1440" alt="Screenshot 2024-06-04 at 2 17 22 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/a67d6a0f-118a-4cd0-a683-325715988c4e">

On the Server Roles tab click "Active Directory Domain Services", on the pop-up window click "Add Features":

<img width="1440" alt="Screenshot 2024-06-04 at 2 19 06 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/48b284a7-a8f2-4efa-8b61-dd946019cda6">

Navigate to the "Confirmation" tab, go ahead and click "Install":

<img width="1440" alt="Screenshot 2024-06-04 at 2 20 53 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f57dd999-4a46-4627-89f7-a04a1b9de624">

Once the installation is complete, go ahead and close this window:

<img width="1440" alt="Screenshot 2024-06-04 at 2 24 32 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/c61f1397-80e4-40d1-8a96-01405c417699">

From here, go to the Notifications flag on the top right of the screen, click it, and you will see that we require Post-deployment Configuration. Click "Promote this server to domain controller":

<img width="1440" alt="Screenshot 2024-06-04 at 2 25 37 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/1b42929e-f128-427d-814e-d9327841d6c4">

On this screen click to "Add a new forest" and give it a name. I chose "mydomain.com" to go with something generic:

<img width="1440" alt="Screenshot 2024-06-04 at 2 27 23 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/79e36730-158c-4611-a8e0-61016c48700e">

On the next tab "Domain Controller Options" go ahead and enter your password then hit "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 2 29 40 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/610a3d3f-16e0-4b2a-b4c9-908b4bf7005d">

Continue clicking "Next" until you reach the "Prerequisites Check" tab, then go ahead and click "Install". This will take some time so hold tight and grab a glass of water:

<img width="1440" alt="Screenshot 2024-06-04 at 2 31 20 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f4134e04-373b-44b0-9084-45f30f9fa238">

The box should automatically restart afterwards:

<img width="1440" alt="Screenshot 2024-06-04 at 2 35 55 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/05117dce-0113-4963-a127-6f5485d924d1">

### Step Two: Creating Admin Account

Now that we are back and logged in, in the search bar let's find "Active Directory Users and Computers" and click on that:

<img width="1440" alt="Screenshot 2024-06-04 at 2 45 38 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/494b9c7b-c011-435e-b740-db684744f1bd">

Once inside, we can now see our domain "mydomain.com" on the left, let's go ahead and right click that to create our first Organizational Unit (AKA OU). Organizational Units (OUs) in Active Directory are like digital filing cabinets that help us organize all our users, computers, and other resources within the network. They make it easier for us to manage everything by grouping similar items together, applying policies, and delegating tasks. This setup not only streamlines our operations but also enhances our security and efficiency.

<img width="1440" alt="Screenshot 2024-06-04 at 2 47 44 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/85bbfc42-356a-4bcc-aa63-6a939b46ec7f">

I am going to name this OU "_ADMINS", the underscore should help it pop so it's easier to spot when working with down the line:

<img width="1440" alt="Screenshot 2024-06-04 at 2 51 10 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/a1446599-6100-4501-bd76-6199d5802c0f">

Back on the "Active Directory Users and Computers" dashboard, we will now see our first Organizational Unit "_ADMINS". Let's create our first admin account by right clicking, selecting "New", then select "User":

<img width="1440" alt="Screenshot 2024-06-04 at 2 57 45 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/945893bf-1b66-4ffc-bafa-524237071bda">

I'm going to create the first user as myself. A good format for creating admins is using "a-" to help you quickly identify admin accounts. Once you have your admin entered, go ahead and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 2 59 33 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f000cffe-415a-4f71-9f00-3ea90ff68248">

Go ahead and enter your users password information. I've selected "Password never expires" for the purpose of convienience in this lab. Then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 3 08 26 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/aa90576f-2e8f-4022-82c2-79df6cb5301c">

The click "Finish":

<img width="1440" alt="Screenshot 2024-06-04 at 3 09 44 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/78ff59e9-3b5b-4694-bb63-281594cdb260">

And perfect! We've created our first user! We can now see "Seve Walliman" inside of our "_ADMINS" OU. Now we need to assign this user administrator rights, so let's right click it and select "Properties":

<img width="1440" alt="Screenshot 2024-06-04 at 3 10 19 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/08bc823a-ba71-438f-9e3f-3af640951abe">

From here, on the pop-up let's select the "Member Of" tab, then click "Add":

<img width="1440" alt="Screenshot 2024-06-04 at 3 13 45 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/aeab9a76-7983-4731-8af5-d602e2a31590">

Under the prompt "Enter the object names to select", let's type "domain admins" and click "Check Names". This should convert our text into "Domain Admins" which is now underlined, now click "OK":

<img width="1440" alt="Screenshot 2024-06-04 at 3 16 15 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/555eab2b-8117-4ff4-9aba-1b59d3a9ec30">

Now click "Apply":

<img width="1440" alt="Screenshot 2024-06-04 at 3 17 36 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/481f84da-4422-4493-82dd-36c7834ccef7">

Then "OK":

<img width="1440" alt="Screenshot 2024-06-04 at 3 18 22 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/b4369a2c-173a-4235-bc74-ec4083cdcaa6">

Success! From here we can click the Windows button on the bottom left of the toolbar, click the profile icon, then "Sign out" so that we can login and continue our configuration through our new user account:

<img width="1440" alt="Screenshot 2024-06-04 at 3 19 28 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/5421cf9e-e4d8-40d3-9334-230f03a543c5">

And that concludes Part Two! In Part Three we will configure RAS/NAT to allow machines we connect to our domain to communicate with the internet using the same external IP address, then DHCP to allow the machines to automatically obtain their internal IP addresses. This is especially useful for our Azure configuration where you are only allowed 3 public IP addresses with a free account.

## Part Three: Configuring RAS/NAT and DHCP
