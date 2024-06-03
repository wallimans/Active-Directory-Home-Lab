# Overview

We will be using the following tools:

- Microsoft Azure
- Windows Server 2019
- Active Directory Services
- Powershell

For this project we are going to spin up an instance of Windows Server 2019 on Azure, configure it as our Domain Controller, then set it up to run Active Directory. This will require creating an IP addressing scheme, two Virtual NICs (one for the interior and exterior networks), as well as configuring RAS/NAT and DHCP to let any host machines communicate with the outside network and automatically obtain their internal IP addresses. Before we deploy a host machine we will run a Powershell script to automatically provision 1,000 users for our practice purposes.

This file will server as the master document containing all the parts and steps, but you can navigate to individual sections as well:

Part One
Part Two
Part Three

## Part One: Configuring Windows Server 2019 Ethernet Settings

### Step One: Installing Windows Server 2019 on Azure

Our first step is creating the virtual machine that we will configure as our domain controller and use for the lab. I've broken this process down [here](https://github.com/wallimans/Home-Lab/tree/main/Virtual-Machines/Microsoft-Azure/Creating-Windows-Server-2019).

## Step Two: Establishing Ethernet Settings

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

And that concludes Part One! We've ensured we have both the interfaces we need to continue and are ready to move onto Part Two.
