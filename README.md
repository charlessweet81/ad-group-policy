<p align="center">
<img src="https://i.imgur.com/pJSsvpx.png" alt=""/>
</p>

<h1>Group Policy & Managing Accounts</h1>
<p>
This tutorial walks you through the process of setting up a Domain Controller (DC) and a Client in Microsoft Azure, showcasing how to configure network connections and test functionality in a controlled virtual environment. By deploying a Windows Server as a Domain Controller (DC-1) and a Windows 10 machine as a client (Client-1), we’ll explore how to configure private networks, assign static IPs, and test connectivity between virtual machines.

Through this step-by-step guide, you’ll gain practical experience in:

- Establishing a domain environment in Azure.
- Configuring DNS settings and testing network connectivity.
- Troubleshooting and ensuring seamless communication between virtual machines.

Let's dive in. 

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory

<h2>Operating Systems Used</h2>

- Windows Server 2022
- Windows 10

<h2>Overview</h2>

  - Step 1: Log into the Domain Controller (dc-1)
  - Step 2: Pick a Random User Account
  - Step 3: Simulate Failed Login Attempts
  - Step 4: Configure Group Policy to Lock Out Accounts
  - Step 5: Test the Lockout Policy
  - Step 6: Observe the Account Lockout in Active Directory
  - Step 7: Unlock the Account
  - Step 8: Reset the Password
  - Step 9: Test the Login
  - Step 10: Disable the Account
  - Step 11: Re-enable the Account
  - Step 12: Observe Logs on the Domain Controller
  - Step 13: Observe Logs on the Client Machine

<h2>Installation Steps</h2>

<h4>Step 1: Log into the Domain Controller (dc-1)</h4>

<img src="https://i.imgur.com/nTMpYVh.png" height="80%" width="80%" alt=""/>

- Open a Remote Desktop Connection (RDP) session or physically access the Domain Controller (dc-1).
- Use domain admin credentials to log in.

<h4>Step 2: Pick a Random User Account</h4>

<img src="https://i.imgur.com/dc07sEq.png" height="80%" width="80%" alt=""/>

- Open Active Directory Users and Computers (ADUC):
  - Navigate to Start > Administrative Tools > Active Directory Users and Computers.
  - Select a previously created user account. For example: johndoe.

<h4>Step 3: Simulate Failed Login Attempts</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- On a client machine or in a testing environment, attempt to log in using the selected user account.
- Enter an incorrect password 10 times.
- Observe that the account is not locked out yet because the lockout policy hasn’t been configured.

<h4>Step 4: Configure Group Policy to Lock Out Accounts</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- Open the Group Policy Management Console (GPMC) on the Domain Controller.
- Start > Administrative Tools > Group Policy Management.
- Navigate to the Default Domain Policy:
- Expand your domain > Right-click Default Domain Policy > Click Edit.
- Set the lockout policy:
  - Go to Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.
- Configure the following settings:
  - Account lockout threshold: Set to 5 invalid login attempts.
  - Account lockout duration: Set to 30 minutes (or your preference).
  - Reset account lockout counter after: Set to 30 minutes.
  - Save and close the GPO editor.
- Force the policy update:
  - Run gpupdate /force on the Domain Controller and the client machine.
 
<h4>Step 5: Test the Lockout Policy</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- On a client machine, attempt to log in with the user account (johndoe) using a bad password 6 times.
- Observe the account lockout error message after the 6th attempt.

<h4>Step 6: Observe the Ticket Lockout in Active Directory</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- In ADUC, navigate to the locked account:
- Locate the user (e.g., johndoe) > Double-click the account.
- Observe that the account is locked (status shown in the account properties).
- Note the "Account is locked out" checkbox.

<h4>Step 7: Unlock the Account</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- In ADUC, unlock the account:
  - Right-click the user > Click Properties > Go to the Account tab.
  - Uncheck Account is locked out and click OK.
 
<h4>Step 8: Reset the Password</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- Reset the user’s password:
- Right-click the user in ADUC > Click Reset Password.
- Enter a new password and click OK.

<h4>Step 9: Test the Login</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- Reset the user’s password:
- Right-click the user in ADUC > Click Reset Password.
- Enter a new password and click OK.

<h4>Step 10: Disable the Account</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- In ADUC, disable the account:
  - Right-click the user > Click Disable Account.
- Test login:
  - Attempt to log in with the disabled account on a client machine.
  - Observe the error message: "Your account has been disabled. Please contact your administrator."

<h4>Step 11: Re-enable the Account</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- In ADUC, re-enable the account:
  - Right-click the user > Click Enable Account.
- Test login:
  - Log in with the account to confirm it works.

<h4>Step 12: Observe Logs on the Domain Controller</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- Open Event Viewer on the Domain Controller:
  - Start > Administrative Tools > Event Viewer.
- Navigate to Windows Logs > Security.
- Look for events related to:
  - Failed logins: Event ID 4625.
  - Account lockout: Event ID 4740.
  - Password changes: Event ID 4723 or 4724.

<h4>Step 13: Observe Logs on the Client Machine</h4>

<img src="https://i.imgur.com/HfQeAYK.png" height="80%" width="80%" alt=""/>

- On the client machine, open Event Viewer:
- Navigate to Windows Logs > Security.
- Look for similar events (e.g., failed logins) tied to the test user account.

Step 13: Observe Logs on the Client Machine
