# Active-Directory-Home-Lab

## Objective
The Active Directory Home Lab project was designed to establish and configure a comprehensive network infrastructure in our Oracle Virtual Box. I will demonstrate how to efficiently add over 1,000 users using PowerShell Scripts for automation.

### Skills Learned
- Installing, configuring, and managing Active Directory Domain Services (AD DS), including creating and managing a large number of user accounts
- Writing and executing scripts to automate the creation and management of users in Active Directory
- Setting up and configuring virtual machines and virtual network environments using Oracle VirtualBox
- Configuring network settings, including IP addressing and DNS, to ensure proper connectivity and secure communication between virtual machines
- Managing server roles and features, performing regular maintenance tasks, and monitoring system performance and health

### Tools Used
- Oracle Virtual Box used for housing our entire network, server, and client machine.
- Windows Server 2019 ISO for installing the server to the Virtual box to house Active Directory.
- Windows 10 Disk Image ISO for installing the client machine to the Virtual box to access the internal network.

## Network Diagram
![Topology](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/b2fc3e21-eb5b-4bd9-9419-6bad4fa515cd)

## Steps
1. Create A Virtual Machine for Windows Server
   
Open your VirtualBox:

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/a8a1821b-97dd-43a0-9cf1-998b8d6323ea)

Click new => Name your VM, select the version "Other Windows (64-bit)," and hit next => Assign at least 2GB of RAM and assign 4 CPU processors, and hit next => next => Finish
Next, open up the settings and select “General” => select “Advanced” and choose “Bidirectional” in the dropdown box for both “Shared Clipboard” and “Drag ‘n’ Drop”

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/f3965915-901c-4ea9-b656-a771bc585d15)

Next, assign two network adapters to the Virtual Machine. Network Adapter 1 will be used to establish a connection with the external network, i.e., the internet. On the other hand, Network Adapter 2 will be designated for the private network, enabling client devices to connect securely.

Network Adapter 1:

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/0ebfcbf6-08c9-42fe-b9da-701062ff65a7)

Network Adapter 2:

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/811ba8c4-4aa1-4604-8787-c9ee27e8814e)

Note: The VM being configured is the one you installed Windows Server 2019 on. It will serve as the domain controller. Remember that the DC is a server that has active directory software installed on it.

Once you're finished setting up the VM settings you can select the server and start it up. Remember to select "input" => "Keyboard" => "insert ctrl-alt-delete" to take you to the login page where you will insert your password.

Click on "Network"

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/578b1584-71a0-4c1f-b73c-0f9cf24c86c2)

Click on "change adapter options". You will be presented with two Ethernet connections, right click one of the ethernet connections => select "Status" => click "Details..." => determine which one is your internal network and external network. Once you have identified your internal and external network, you should rename the networks to easily identify them later.

Shown Below:

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/bb750b63-c6fc-43ef-8881-20ac4c199f59)

If you want you can rename the system by right clicking "Windows Start" => click "System" => click "Rename this PC" to DC or Domain Controller.
Once done you can restart the machine

