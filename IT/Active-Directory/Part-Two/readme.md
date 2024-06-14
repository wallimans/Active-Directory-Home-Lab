## Part Two: Configuring RAS/NAT and DHCP

RAS (Remote Access Service) and NAT (Network Address Translation) are tools that help us manage how our network connects to the outside world. RAS allows our remote employees to securely access the company's network from anywhere, like they were in the office. NAT, on the other hand, lets multiple devices on our internal network share a single public IP address to access the internet, keeping our internal IPs hidden and adding a layer of security. Together, they ensure our network is accessible and secure for everyone who needs it.

Without RAS (Remote Access Service) and NAT (Network Address Translation), our network would face significant challenges. Remote employees wouldn't have a secure way to access company resources from outside the office, limiting flexibility and productivity. Additionally, without NAT, every device on our network would need its own public IP address to access the internet, which is impractical and costly. It would also expose our internal network directly to the internet, increasing security risks. Essentially, RAS and NAT are crucial for maintaining a secure, efficient, and flexible network environment.

### Step One: Configuring RAS/NAT

To start the RAS configuration, let's start as before on the Server Manager Dashboard and select "Add roles and features":

<img width="1440" alt="Screenshot 2024-06-04 at 3 59 48 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/c30d347c-4f4e-4d50-9fd0-9aa2f7a14ae9">

Click "Next" until we are on the "Server Selection" tab and make sure we have the correct server selected. Then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 01 12 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/143bdb8e-e21d-4b3f-9412-dabacbe009ca">

On the "Server Roles" tab we will select the "Remote Access" box then click "Next" a few times until we are on the "Role Services" tab where we will select the "Routing" checkbox, and on the pop-up click "Add Features".

<img width="1440" alt="Screenshot 2024-06-04 at 4 04 09 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/4a91e0e0-b817-453f-ac6c-4e6e164bb616">

Then click "Next" until we are on the "Confirmation" tab and select "Install":

<img width="1440" alt="Screenshot 2024-06-04 at 4 05 02 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/cabe3624-62cd-4374-beeb-5baf62d40aa3">

Once the installation is complete we can click "Close":

<img width="1440" alt="Screenshot 2024-06-04 at 4 08 34 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/d7e2fc77-b168-44b9-bac5-7f6bae84b4d3">

Now, back on the Server Manager Dashboard, we will select "Tools" from the top-right corner and navigate to "Routing and Remote Access":

<img width="1440" alt="Screenshot 2024-06-04 at 4 09 18 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/282c64da-1d69-4579-956d-a3e96ff39a89">

From here we will see our local server (mine is named "ADBox"), let's right click it and select "Configure and Enable Routing and Remote Access":

<img width="1440" alt="Screenshot 2024-06-04 at 4 10 47 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/32c2c1bf-d1c0-46da-bab4-9eb8e0b525a9">

Once inside the Routing and Remote Access Server Setup Wizard, click "Next", choose "Network address translation (NAT)", then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 13 48 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/389b254b-1c60-48f4-8110-9cd7e03fee28">

From here we will select our "_INTERNET_" interface that we renamed eariler for easy identification, then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 14 11 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/cf08202b-8986-469a-a36d-6c22c63aef0c">

Normally this should configure itself without a problem, but perhaps because we are running this process inside Azure we have encountered our first hiccup! We've recieved the prompt: "Remote Access Service is unable to enable Routing and Remote Access for the probable reason like: unable to open ports for Routing and Remote Access in Windows Firewall service. In this case, RAS may not accept vpn connections. User Action: Manually open the port of Routing and Remote Access in the windows firewall."

So that's what we'll do! Manually configure a firewal rule to allow the port:

Type "Windows Defender Firewall" in your search bar, click it, then select "Advanced settings":

<img width="1440" alt="Screenshot 2024-06-04 at 4 24 29 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/2de1e23a-1c58-4b18-b89e-5a2a8d69a33f">

On the lefthand pane we will see "Inbound Rules", let's right click that and select "New Rule..."

<img width="1440" alt="Screenshot 2024-06-04 at 4 25 13 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/81f1d779-f0e5-4442-8ed7-558e8aa7d293">

Under "Rule Type" we will select "Port" then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 26 23 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/7b542509-76d6-4660-92c8-2753dc07f58a">

Here we can specify the ports we would like to allow through the firewall. Some common Remote Access ports include:

- PPTP: 1723
- L2TP/IPsec: 1701, 500, 4500
- SSTP: 443

So we will go ahead and enter those, then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 30 27 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/79ae7317-d123-4eef-8f2d-7a592947eb7f">

Select "Allow the connection" then hit "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 30 53 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/cee95df3-8a83-410b-8cda-90ce14107199">

Let's apply this rule to everything, then hit "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 31 23 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/2f80354a-473e-48ea-8f6b-df3633cf036a">

Let's give it a name, I chose "Remote-Access-TCP", then hit finish:

<img width="1440" alt="Screenshot 2024-06-04 at 4 32 15 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/6e060f85-43e3-496e-9926-394dc579e9ff">

I've went ahead and went through these identical steps one more time, but this time selecting UDP for ports 500, 4500 which can also sometimes use UDP.

Closing our Windows Defender Firewall we can navigate back to our Routing and Remote Access pane from before, and everything should look good! We are ready to continue onto the next step!

<img width="1440" alt="Screenshot 2024-06-04 at 4 37 51 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/5ba1e49c-3fa8-4e70-b8df-ce2c69d742f4">

### Step Two: Configuring DHCP

DHCP (Dynamic Host Configuration Protocol) is a network management protocol that automatically assigns IP addresses and other network configuration settings to devices on a network. This eliminates the need for manual configuration, ensuring devices can communicate with each other efficiently and reducing administrative overhead.

Let's setup DHCP for our internal network! As with previous steps, let's start on the Server Manager Dashboard and select "Add roles and features:"

<img width="1440" alt="Screenshot 2024-06-04 at 4 40 39 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/ceaf87b3-d221-4b59-a08f-110fcd0659de">

Go ahead and click "Next" until we reach our "Server Selection" tab and select our server, then click "Next": 

<img width="1440" alt="Screenshot 2024-06-04 at 4 48 11 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/421c8c69-1062-4d9f-b180-510fb2b71e0a">

On the "Server Roles" tab we will check the "DHCP Server" box, then on the pop-up click to "Add Features":

<img width="1440" alt="Screenshot 2024-06-04 at 4 50 08 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/ac6c4e5f-c687-4a2d-ae09-07b2efb4e6be">

Then go ahead and click "Next" until we reach the "Configuration" tab where we will click "Install":

<img width="1440" alt="Screenshot 2024-06-04 at 4 50 42 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/170a7281-d5c8-4ad3-b964-1f8f9e8c57ed">

After the process finished, go ahead and click to "Close":

<img width="1440" alt="Screenshot 2024-06-04 at 4 52 20 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f354224d-7033-4632-ac9b-e15028a7ef73">

Now we will navigate to "Tools" then click on "DHCP" to enter the configuration process:

<img width="1440" alt="Screenshot 2024-06-04 at 4 53 09 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/1016c895-762d-4f7d-8128-3ecaa062684a">

Once inside our DHCP configuration panel we can click to expand our server, then right click on "IPv4" to enter the configuration wizard:

<img width="1440" alt="Screenshot 2024-06-04 at 4 55 21 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f6a5aca9-3f0e-4835-88ff-55f2d03c8c5e">

Inside our Configuration Wizard, let's click "Next" until we reach the "Scope Name" page, where we will name the scope. It's a good idea just to name the scope after the range it will provide, so in our case that is 192.168.10.100-200. Then go ahead and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 4 58 54 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/becbc490-3cdc-479d-b339-ecaded46b3c1">

On the next page we will configure our IP address range that DHCP will hand out to devices that join the domain. We will also configure our length/subnet mask, which is a bit outside of the scope of this demonstration, but in basic terms subnets divide a larger network into smaller, more manageable sections, or sub-networks. This helps improve network performance and security by controlling traffic flow and isolating segments of the network. Each subnet operates under a common subnet mask, which determines the range of IP addresses that belong to that subnet.

We've chosen a length of 24, which is a subnet mask of 255.255.255.0. Go ahead and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 02 07 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/2df43520-7905-407a-ad30-46d0914dc8e8">

On the next page we will be given the option to add exclusions to our address range. An exclusion range in a DHCP scope is used to reserve specific IP addresses within the range that DHCP would normally assign, preventing them from being automatically given to devices. This is useful for IP addresses that need to be manually assigned to certain devices like servers, printers, or other network equipment that require a fixed IP address. By creating an exclusion range, you ensure these critical devices always have the correct, reserved IP addresses without conflicts. This doesn't apply to us, so we'll click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 08 58 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f6f00e72-1e1b-43ff-967c-610aa34e2765">

We will then be able to configure our lease duration. For the purpose of our lab, the default of 8 days should be fine. Go ahead and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 13 33 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/f1b3df62-67d0-40cd-baec-0a85cb1b7067">

On the next page, let's select "Yes, I want to configure these options now", then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 13 48 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/18adb91c-3125-4af2-9293-efddd464feb3">

Next we will configure our Default Gateway. A default gateway is the network device, typically a router, that serves as the access point or IP router that a networked device uses to send information to devices on other networks or the internet. It acts as an intermediary that routes traffic from a local network to external networks, ensuring communication beyond the local subnet.

The default gateway for this network will be the Internal NIC we configured on the server, so in my case 192.168.10.1. I've gone ahead and typed that in and clicked "Add". Then we can click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 16 55 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/93b9b28f-457d-4110-aa4e-a216f7bb76e6">

On this page we can select which domain we want computers on our network to use for DNS name resolution.

DNS (Domain Name System) is like the internet's phonebook—it translates human-readable domain names (like www.example.com) into numerical IP addresses that computers use to identify each other on the network. It's a crucial system for navigating the internet because it allows us to access websites, send emails, and perform other online activities using familiar domain names instead of remembering long strings of numbers.

When we configured Active Directory, the server automatically configured itself as the DNS server, so in this instance we should leave this option as "mydomain.com" and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 21 32 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/81920215-d1c6-4a1d-9fe7-a48d6c28d1f2">

On this page we can configure WINS Servers.

WINS (Windows Internet Name Service) is an older network service used primarily in Windows environments to resolve NetBIOS names to IP addresses. It's similar to DNS but specifically designed for older Windows-based networks. WINS servers help in identifying and locating computers and resources on a network by providing a centralized database for mapping NetBIOS names to IP addresses. However, with the prevalence of DNS and modern networking technologies, the need for WINS has diminished, and it's often only used in legacy environments or for compatibility with older applications.

Being an older service, we don't really need to set this up, so go ahead and click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 24 16 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/237202d6-70c9-48c4-aabf-b6cb58b0afcc">

On this page let's select "Yes, I want to active this scope now" then click "Next":

<img width="1440" alt="Screenshot 2024-06-04 at 5 24 54 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/087ddc19-ebbf-4828-b167-a5c7ce688266">

Then click "Finish"!:

<img width="1440" alt="Screenshot 2024-06-04 at 5 25 48 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/d07c92c5-9506-4d0f-a744-70f07710e65a">

Back in the DHCP dashboard, let's right click our domain and select "Authorize":

<img width="1440" alt="Screenshot 2024-06-04 at 5 27 52 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/0eee9f7f-e503-45e9-9e54-6fd94cfbb20f">

Then let's right click it again and select "Refresh":

<img width="1440" alt="Screenshot 2024-06-04 at 5 28 14 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/1d38613c-6997-4ce6-906d-c2d0d5772531">

And that concludes part three!
