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
