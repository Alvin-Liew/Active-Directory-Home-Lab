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

2. Setting up the Internal and External Networks

Click on "Network"

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/578b1584-71a0-4c1f-b73c-0f9cf24c86c2)

Click on "change adapter options". You will be presented with two Ethernet connections, right click one of the ethernet connections => select "Status" => click "Details..." => determine which one is your internal network and external network. Once you have identified your internal and external network, you should rename the networks to easily identify them later. 

Shown Below:

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/bb750b63-c6fc-43ef-8881-20ac4c199f59)

Right click the "INTERNAL" network => click “Properties” => click “Internet Protocol Version 4 …” => click the “Properties” option below it again => select “use the following IP” and use the details in our network design figure above for the set up as follows:

IP: 192.168.0.1

Subnet Mask: 255.255.255.0

Default Gateway: <empty> (the DC will serve as the default gateway)

Preferred DNS: 127.0.0.1 (Loop-back-IP address // When we install AD DS, DNS is automatically installed and this server will use itself as the DNS server)

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/0e52df89-b407-4c09-a5a9-82c42161e20a)


If you want you can rename the system by right clicking "Windows Start" => click "System" => click "Rename this PC" to DC or Domain Controller.
Once done you can restart the machine

3. Setting up Active Directory Domain Services
   
Brief theory, There are two main logical network designs:

Workgroup Environment: This is similar to your home network, except that instead of being replicated throughout the devices in the network, a user's credential (password and username) is saved in a database called Security Account Manager (SAM) that is exclusive to the user's device. In this manner, the user's account is formed on that device, and only they can log on to it. No other user can connect on to it. It's a fairly excellent technique to keep your personal device secure, but managing individual devices for password resets and user account creation in a commercial setting with, say, a thousand and one devices, is a major hassle.
Domain-based Environment. To centrally handle user and computer account creation, password resets, and authentication, businesses need a domain-based system. In this case, PCs are connected to the domain and users are established in the domain controller. Unlike Workgroup accounts, these accounts exist in the domain controller rather than on specific devices. In this manner, a user inside the domain may get to any computer inside the domain, provide their login information, and receive authentication to access any machine.

The program that is installed on a Windows server and allows management of users and machines is called Active Directory Domain Services. With the desired DNS setup from the previous step, the AD publishes a large number of DNS entries to the DC. We will install AD DS on the Server 2019 in the ensuing stages.

Click on the Windows icon, and click on “Server Manager”. Click on the “Add Role & Feature” option, => Next => Role-based or feature-based installation => select the “Primary-DC” server => select the “Active Directory Domain Services” => Add features => Next (3X) => Install.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/94210f5f-1f0d-4ec8-8c7c-74bb43675d30)

We installed the AD DS program in the preceding stage. The server will be promoted to a domain controller in order to finish the post-deployment setup. Upon completion of this process, a DNS is installed automatically. Click the yellow triangle caution indicator => and choose "Promote this server to a domain controller" to do this.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/9991ddc5-635b-4b62-bc82-210020bce2ea)

Select "Add a new forest." mydomain.com is the root domain name => Select a passcode => Next (4x) => After installation, an automated server restart will occur. You'll have to sign in once more.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/4f99a39c-f899-4c59-8adf-e7d81ed3e285)

4. Creating a dedicated domain Administrator account

Instead of utilizing the built-in admin account you have been using to access the server, it is best practice to create a separate administrator account.

Click "Start" => "Active Directory Users and Computers" which is located under "Windows Administative Tools"  

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/b0da8428-7db8-4b5d-96c4-df4c81e9ff25)

Right-click "mydomain.com" Novel => New => Organizational Unit => You may call it "Admin" => Select "Admin" from the menu => new user => I'll use Alvin Liew for the first and last names. Use your own name -> a-liew is the user login name ('a' for the admin account, first name initial, last name convention) => Next => enter your password for login => Verify that the password never expires for the sake of this exercise => Subsequent => Completion.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/368f0b2f-497e-4899-a5e0-dafe3fa2aa05)

A user has been created, however this user has to be elevated to administrator. Click "properties" => "Member of" => Add => enter "Domain Admins" in the textbox => OK => Apply after performing a right-click on the user.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/2b3fe35a-8d7d-450d-9d8a-3102defeef87)

We can now sign out of the default admin account and sign in using the domain admin user account we just created.

Click on the Windows icon => click on the Administrator icon => click “sign out” => and sign in with the new account.

5. Install Remote Access Services (RAS) and Network Address Translation (NAT) on the DC

Our network is set up to mimic a business setting, with traffic from clients on our private network or local area network (LAN) being sent to the internet via the domain controller, which is a single IP address. The NAT and RAS gateways make this feasible.

To install RAS and NAT, navigate to the server manager => click “Add roles & features” => Next => Role-based or feature-based installation => select the “DC” server => click “Remote Access” => Next (3x) => Select “Routing” => Add feature => Next (3x) => Install.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/eddcdb54-6188-404b-8fdb-a97d4a3041d2)

Once the installation is completed, click on “tools” => click on “Router and Remote Access Service” => Right-click “DC (local)” => click “Configure and enable RRAS” => Next => Select Network Address Translation => Next => If your network interfaces don’t show up, cancel and repeat the process => Select the internet => Next => Finish.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/b0a923c6-e834-4b80-8d01-b48fbce6402e)

With the installation of RAS and NAT completed computers on our LAN will be able to reach the internet once the DHCP server is installed.

6. Install Dynamic Host Configuration Protocol (DHCP) server

A mechanism called DHCP is used to dynamically allocate IP addresses to networked devices. We won't go into great depth regarding this protocol's operation, but in general, IP addresses are issued depending on the scope (range) of selected IP addresses as well as the mode (Automatic, Dynamic, and Fixed) defined on the DHCP server.

As can be seen from the network architecture above, we have IP addresses reserved in the 172.16.0.100–200 range. This implies that an IP address within this range will be "offered" to any machine wanting to join our network. Now, that's quite a bit of theory.

Click "Add roles & features" as per normal => Following => Installation based on features or roles => Choose the "DC" server. Choose the DHCP server. Next (3 times) => Add features Put in.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/1efc0292-41fb-455b-863d-480e7b07b0eb)

Once the installation is completed, click on “close”. Next is to configure the installed DHCP server with the scope as discussed.

Click “Tools” => DHCP => Click “Primary-DC” => Rightclick on “IPv4” => New scope => use 192.168.0.150–250 as the scope name => Start IP address: 172.16.0.100 => End IP address: 172.16.0.200 => subnet mask of /24 or 255.255.255.0 => No exclusion in our case so click “Next” => Lease duration is case dependent, so leave the default => Click yes to configure DHCP options — this specifies how computers joining the domain will access the internet. In this case, our DC is the option, so click next and Enter 172.16.0.1 => click “Add” => Next (3x) => Finish.

The IPv4 is currently displaying a red arrow, We need to authorize the DC and refresh. So right-click “Primary-DC” => click authorize => right-click again and click refresh and both IPv4 and IPv6 will turn green. As you notice the address lease is empty and this is where the windows 10 client machine will go

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/6a17b1a9-acbe-416f-a170-cf067e2f8c8f)

6. Automating the creation of 1,000 users using powershell script

Click "Start" => "Windows PowerShell ISE" which is located under "Windows PowerShell"

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/11812f5c-774b-4ee8-af17-9d28a30977e8)

Click "new script" and paste the script as follows:

# ------------------------------------------------------ #
$PASSWORD_FOR_USERS   = "UserPassword1"

$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {

    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}

Before running the code you must generate 1000 random first and last names. I used Chatgpt and inserted all of them into a text document. Make sure to rename the text document to names or the code won't work.

After creating the text document, You would need to locate the file using this command: "c:\users\a-liew\desktop", this is if your text document is on the desktop. Type ls and make sure that the file is stored in the location. Make sure to replace a-liew with whatever account you are logged into

Once you have located the file you can run the code

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/5b89a87f-0f24-4206-b418-6677c3534251)

Once the script is finished, You will start to notice in your "Users" file located in "Active Directory User and Computers" that there are 1000 users now.

(Remember that "$PASSWORD_FOR_USERS   = "UserPassword1"" is what we are setting as the passwords for all the accounts which you will need later)

7. Create a Virtual Machine for your Windows Computer

Click new => Name your VM, select the version "Windows 10 (64-bit)," and hit next => Assign at least 4GB of RAM and assign 4 CPU processors, and hit next => next => Finish
Next, open up the settings and select “General” => select “Advanced” and choose “Bidirectional” in the dropdown box for both “Shared Clipboard” and “Drag ‘n’ Drop”

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/8ebb5129-50f8-4289-8fc7-23d44514f18e)

Next, Assign the network adapter 1 to the internal network establishing a connection from the client computer to the server using a virtual network.

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/730e7dee-99a1-44fa-a544-73e590e6bae3)

Once you're finished setting up the VM settings you can select the windows 10 machine and start it up. Setup windows 10 and make sure to select windows 10 pro when setting it up. 

When the installation is completed, you will need to setup the machine for an organization and select "Domain join instead" => Fill out the name with "User" => and leave the passwork empty => Once you have load up to the desktop screen we will start a network test => search of CMD => start it up => ping www.google.com => start another ping to mydomain.com

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/23056f08-31b0-4ede-92a2-d48bb700ff68)

As you can see that www.google.com resolved which means that our DNS server is working and since we can ping to the internet that means the whole infrustructure is working. This shows that we have connectivity all the way the default gateway which is the domain controller and the domain controller is properly natting it and forwarding it out to the internet and properly come back as an echo reply.

We can change the name of the windows computer to Client1 following the network diagram by right clicking "Windows Start" => click "System" => click "Rename this PC (advanced)" => click "Change..." => Rename computer name to "CLient1" => select "Domain:" and enter: "mydomain.com" => click ok => enter the admin account we created awhile back: (user: a-liew) => restart now

Once this is done if you go back to the server machine and navigate through the server manager to tools => Click DHCP => click the dropdown bar for "IPv4" => click "Scope [172.16.0...0]" => click address leases we can see that we have one lease from our client computer

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/c71caaea-cf5c-4352-ba86-cf0729825630)

If you navigate to your Computers by Clicking "Start" => "Active Directory Users and Computers" which is located under "Windows Administative Tools" => click the dropdown bar for "mydomain.com" => click "Computers" you will find our client computer there as well

![image](https://github.com/Alvin-Liew/Active-Directory-Home-Lab/assets/105011531/64ea56f0-72bf-4904-8c39-aa2558e7e7c9)

With all this setup you can go back to your client machine and login as any of the users we created using the powershell script

## Conclusion
We have successfully completed the setup and configuration of our network infrastructure. By installing and configuring the necessary software, such as DHCP, and utilizing an architectural design blueprint, we have ensured smooth network connectivity and IP address assignment for clients. By automating user creation through a PowerShell script, we have streamlined administrative tasks. With the Windows 10 virtual machine running as our client, we have verified the network’s functionality. By adding the client machine to the domain, we have integrated it with our Active Directory setup. Overall, this home lab implementation demonstrates a successful deployment of network and domain services, allowing for efficient management and user authentication.
