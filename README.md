<h1>Home Lab for Windows Server 2022 with Active Directory and Group Policy</h1>

<h2>Description</h2>

<b>Active Directory (AD) is a database and set of services that connect users with the network resources they need to get their work done.

Domain Name System (DNS) is the phone book of the internet. It’s the system that converts website domain names (hostnames) into numerical values (IP addresses) so they can be found and loaded into your web browser

The Dynamic Host Configuration Protocol (DHCP) is a network management protocol used on Internet Protocol (IP) networks for automatically assigning IP addresses.

A Group Policy Object (GPO) is a virtual collection of policy settings. Group Policy settings are contained in a GPO. A GPO can represent policy settings in the file system and the Active Directory</b>

<br />

<h2>Tools and Utilities Used</h2>

- <b>AD, DNS, DHCP & GPO</b>
<h2>Environments Used </h2>

- <b>VirtualBox</b>
- <b>Windows Server 2022</b>
- <b>Windows 10 & 11</b>
- <b>PowerShell</b>

<h2>Network diagram</h2>

<img width="619" alt="image" src="https://github.com/user-attachments/assets/913d9d82-b330-4d02-81e1-8c2f373bc5ed">


<h2>Program walk-through:</h2>

<b>Install VirtualBox </b>

<img width="512" alt="image" src="https://github.com/user-attachments/assets/93ab57e6-eb9d-4ef3-91c8-5817cc818366"><br>
<img width="363" alt="image" src="https://github.com/user-attachments/assets/87224c5c-fe75-4165-9edb-62026943ce00"><br>
<img width="419" alt="image" src="https://github.com/user-attachments/assets/4c1e93df-52c2-4ea9-a33f-d2dc4c5ec868"><br>
<img width="373" alt="image" src="https://github.com/user-attachments/assets/fb723045-2bf5-4edc-b9d3-27f262219f16"><br>

<b>Setting up a Windows Server 2022 VM in VirtualBox</b><br>

<b>*The process would be the same with Windows 10 and 11 *</b>

<img width="380" alt="image" src="https://github.com/user-attachments/assets/84c00eda-7470-4ece-ab38-9d38ad09a044"><br>
<img width="370" alt="image" src="https://github.com/user-attachments/assets/8680c3ae-7178-4883-8b8a-c0a6b1b5cce4"><br>
<img width="373" alt="image" src="https://github.com/user-attachments/assets/351bd3c2-5888-402b-9e0a-0a60b4af6246"><br>
<img width="371" alt="image" src="https://github.com/user-attachments/assets/b672bca5-d862-462a-92ae-4c018e56c927"><br>
<img width="370" alt="image" src="https://github.com/user-attachments/assets/65b040f2-f93a-44b3-93c9-66b54cbc2482"><br>
<img width="328" alt="image" src="https://github.com/user-attachments/assets/0f81d797-4aa1-49ec-a3b0-3bfd94a6fb92"><br>
<img width="392" alt="image" src="https://github.com/user-attachments/assets/a90c845d-6436-4018-954f-48f73f74fb64"><br>


<b>Installation of Active Directory</b>

<img width="380" alt="image" src="https://github.com/user-attachments/assets/8999248e-5ea9-4e13-baa0-3ceb364bd8d4"><br>
<img width="406" alt="image" src="https://github.com/user-attachments/assets/3d04b623-5461-4d4d-a4a5-d23694f49e4b"><br>
<img width="404" alt="image" src="https://github.com/user-attachments/assets/0efbc3db-8559-493f-8d7a-22e3db6be567"><br>
<img width="401" alt="image" src="https://github.com/user-attachments/assets/f5caf8cc-7f8f-4052-99ba-e1ec129de54b"><br>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/f82c5c60-e3d4-4c72-a549-7921f43dea48"><br>
<img width="467" alt="image" src="https://github.com/user-attachments/assets/5bfb5200-2e30-44ba-a0fc-02a53e631ca1"><br>
<img width="400" alt="image" src="https://github.com/user-attachments/assets/b06ac8b2-70df-43d8-9412-4241a0501a9d"><br>
<img width="401" alt="image" src="https://github.com/user-attachments/assets/1061b582-10ab-419f-b9b1-e3137567279a"><br>
<img width="395" alt="image" src="https://github.com/user-attachments/assets/16633e22-520d-4478-a46d-f7417257185f"><br>
<img width="397" alt="image" src="https://github.com/user-attachments/assets/dede394d-484c-475d-87cb-e0a7ca1c0bae"><br>

<b>Use PowerShell to generate 1,000 users with random names </b>

$PASSWORD_FOR_USERS   = "Password123"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000
Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($($count % 2) -eq 0) {
            $name += $consonants[$(Get-Random -Minimum 0 -Maximum $($consonants.Count - 1))]
        }
        else {
            $name += $vowels[$(Get-Random -Minimum 0 -Maximum $($vowels.Count - 1))]
        }
        $count++
    }

    return $name

}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $firstName = generate-random-name
    $lastName = generate-random-name
    $username = $fisrtName + '.' + $lastName
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    New-AdUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_EMPLOYEES,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
    $count++
}

<b> Generate password as 'Password123' </b>

$PASSWORD_FOR_USERS   = "Password123"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
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

<img width="949" alt="image" src="https://github.com/user-attachments/assets/8ee78399-c5a4-4658-ba1c-99226166bc91"><br>

<b>Verify names on AD </b></br>
Click on Tools -> then Active Directory Users and Computers

<img width="302" alt="image" src="https://github.com/user-attachments/assets/d326bf55-abe6-422d-a404-8753ff48b377"><br>

<b>Create a new user name </b><br>
Right click->New->User

<img width="425" alt="image" src="https://github.com/user-attachments/assets/6f4e2ae1-1208-4d56-b43d-f1986ba1fa30"><br>

![image](https://github.com/user-attachments/assets/8b8a0dc3-39ff-46f5-b800-11967d348a69)<br>

<b>Reset a Password </b><br>
Search username of the user who forgot their password<br>
<b>Right-click -> Reset Password</b>
<img width="401" alt="image" src="https://github.com/user-attachments/assets/8591f719-a3cc-42ae-a8c4-981f0e15cbc6"><br>

<b>If you want to deactivate a user account</b>
<b>Right-click -> Disable Account</b>

<b>Group Policy</b><br>
Search for Edit Group Policy
<img width="950" alt="image" src="https://github.com/user-attachments/assets/a9189283-7429-4e2b-ba3d-1d8217f0cadd"><br>
<h2>Examples</h2>
<b>a. Users should change their lock screen to display a specific background featuring the company logo</b>
<img width="517" alt="image" src="https://github.com/user-attachments/assets/9baa582c-71f9-4e89-925a-c836f097dbd9"><br>

<b>b. Add password policy i.e minimum password length and enable complexity</b>
<img width="514" alt="image" src="https://github.com/user-attachments/assets/5095a0f6-10ef-487a-a66b-ad19c9fda6a1"><br>

<b>c. Deploying scripts via Group Policy can help automate tasks like software updates, logon notifications, and system configurations. Here are the step-by-step instructions for deploying a startup script using Group Policy.</b>

<b>Step 1: Prepare the Script</b>
First, create or obtain the script you want to deploy. Make sure it is saved with the appropriate extension (e.g., .bat for batch files, .vbs for VBScript, .ps1 for PowerShell scripts).

<b>Step 2: Open Group Policy Management Console</b>
Press Win + R, type gpmc.msc, and press Enter to open the Group Policy Management Console (GPMC).

<b>Step 3: Create a New Group Policy Object (GPO)</b>
In the GPMC, right-click on the Organizational Unit (OU) where you want to apply the policy and select Create a GPO in this domain, and Link it here.
Give the new GPO a descriptive name, such as "Startup Script Deployment," and click OK.

<b>Step 4: Edit the GPO</b>
<b>Right-click on the newly created GPO and select Edit.</b>

<b>Step 5: Navigate to the Scripts Section</b>
In the Group Policy Management Editor, navigate to Computer Configuration > Policies > Windows Settings > Scripts (Logon/Logoff).
Double-click on Startup in the right pane.

<b>Step 6: Add the Script</b>
In the Startup Properties window, click on Add.
In the Add a Script window, click on Browse to locate the script you prepared.
Navigate to the location of the script, select it, and click Open.
Optionally, you can add script parameters if needed.
Click OK to add the script.
The script will now be listed in the Startup Properties window. Click OK to close the window.

<b>Step 7: Apply and Close</b>
Close the Group Policy Management Editor.
The GPO is now configured and linked to the selected OU. It will be applied to all computers within that OU at their next startup.

<b>Step 8: Verify the Script Execution</b>
Restart a computer within the OU to verify that the script executes at startup.
Check the script's output or system logs to ensure it ran successfully.

By following these steps, you can deploy scripts to multiple computers in your network using Group Policy, automating various administrative tasks.

<img width="509" alt="image" src="https://github.com/user-attachments/assets/398e610f-1bc2-44c9-9b98-1706e32170f2">


<h2>Configuring DHCP</h2>

Step 1: Install the DHCP Server Role
Open Server Manager:
In Server Manager, click on Manage and select Add Roles and Features.
Role-based or Feature-based Installation:
Choose the server you want to install DHCP on and click Next.
Select Server Roles:
Check DHCP Server and click Next.
Review the installation selections and click Install.
Complete the Installation:

Step 2: Configure the DHCP Server
Open DHCP Management Console:
Go to Server Manager, click on Tools, and select DHCP.
Authorize the DHCP Server:
In the DHCP console, right-click on the server name and select Authorize. Wait for the server to be authorized. It may take a few minutes.
Create a New Scope:<br>
Right-click on IPv4 and select New Scope.
Scope Name and Description:<br>
<img width="470" alt="image" src="https://github.com/user-attachments/assets/297cc50f-5dee-4195-968c-f2dd959bcc6f"><br>
Enter a name and description for the scope, then click Next.
Define IP Address Range:<br>
<img width="260" alt="image" src="https://github.com/user-attachments/assets/f11ba1e1-9d98-4d6a-ba9d-bd820454190d"><br>
Enter the start and end IP addresses for the scope, then click Next.
Configure Lease Duration:
Set the lease duration as required (e.g., 8 days), then click Next.<br>
<img width="260" alt="image" src="https://github.com/user-attachments/assets/da484e38-454e-4b9b-9e54-30e69b82c820"><br>
Specify DHCP options like the default gateway (router) and DNS servers. Click Next after entering the details.
Choose to activate the scope immediately, then click Next and Finish.<br>


Step 3: Verify and Monitor DHCP Configuration
Check Scope Status:
In the DHCP Management Console, right-click on the scope and select Properties to verify settings and status.<br>
<img width="344" alt="image" src="https://github.com/user-attachments/assets/b831164f-1b02-4245-9df9-9ddfb92fd238"><br>



In summary, Active Directory (AD), Dynamic Host Configuration Protocol (DHCP), and Group Policy Objects (GPOs) are integral components of a well-functioning network infrastructure. Active Directory serves as a central database and service framework that connects users to necessary network resources, facilitating access and management. Dynamic Host Configuration Protocol automates the assignment of IP addresses within a network, simplifying device connectivity and management. Lastly, Group Policy Objects provide a way to centrally manage and enforce policy settings across an organization, ensuring consistent configurations and compliance. Together, these technologies form the backbone of network management and security, streamlining operations and enhancing connectivity across the digital landscape

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
