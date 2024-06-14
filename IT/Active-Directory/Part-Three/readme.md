## Part Three: Provisioning 1,000 Users w/ Powershell

For this next part we will be using the Powershell scripting language to provision some Active Directory Accounts for us to practice with.

PowerShell is a tool from Microsoft that helps you automate tasks and manage computer settings easily. Think of it like a supercharged command prompt that lets you write simple scripts to handle repetitive jobs and control your system more efficiently. It works on both Windows and other operating systems, making it versatile for various tech tasks.

At the time of writing this walkthrough I am by no means fully proficient with Powershell, but I do have enough familiarity with other programming languages to understand what something does when I break it down line by line. With enough Google-fu I was able to cobble together something that works for our purposes. The script looks like this:

```
$PASSWORD_FOR_USERS   = "Password1"
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
```

In essence this script creates users based on a .txt file containing a large list of names and ensures that each user is set up with a default password, placed in a specific organizational unit "_USERS", and has a username generated from their first and last names. Let's break it down section by section!

**1) Setting Up Variables**

`$PASSWORD_FOR_USERS = "Password1"`

This line sets a variable called `$PASSWORD_FOR_USERS` with the value `"Password1"`. This will be the default password for the new users.

`$USER_FIRST_LAST_LIST = Get-Content .\names.txt`

This line reads a file named names.txt that contains a list of names, and stores each line from the file into the $USER_FIRST_LAST_LIST variable.

**2) Preparing the Password**

`$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force`

This line converts the plain text password "Password1" into a secure string format that is required by the New-AdUser cmdlet. A secure string is a way to store sensitive information, like passwords, in a protected format that is not easily readable or accessible.

**3) Creating the "_USERS" Organizational Unit**

`New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false`

This line creates a new Active Directory Organizational Unit (OU) named _USERS, which is not protected from accidental deletion.

**4) Looping Through the Names**

`foreach ($n in $USER_FIRST_LAST_LIST) {`

This line begins a loop that processes each name in the `$USER_FIRST_LAST_LIST`.

**5) Splitting the First and Last Names**

```
$first = $n.Split(" ")[0].ToLower()
$last = $n.Split(" ")[1].ToLower()
```

These lines ensure that each name in the list is split into a first and last name. The names are then converted to lowercase.

**6) Generating the Username**

`$username = "$($first.Substring(0,1))$($last)".ToLower()`

This creates a username by taking the first letter of the first name and combining it with the entire last name, all in lowercase.

**7) Printing the Username**

`Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan`

This prints the message "Creating user: "[username]" with a black background and cyan text, so you can see which user is being created while it happens.

**8) Creating the Active Directory Users**

```
New-AdUser -AccountPassword $password `
           -GivenName $first `
           -Surname $last `
           -DisplayName $username `
           -Name $username `
           -EmployeeID $username `
           -PasswordNeverExpires $true `
           -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
           -Enabled $true
```

This command creates a new Active Directory user with the following details:

- Password: Uses the secure string password.
- Given Name: The first name.
- Surname: The last name.
- Display Name: The generated username.
- Name: The generated username.
- Employee ID: The generated username.
- Password Never Expires: Sets the password to never expire.
- Path: Places the user in the _USERS organizational unit.
- Enabled: Sets the account to be enabled.

And that's the script!

So let's go ahead and connect back to our server and open Powershell and execute it! On our desktop, search "Windows Powershell ISE" and right click to run as administrator:

<img width="1440" alt="Screenshot 2024-06-05 at 1 28 53 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/e7f21095-d2f6-4cbd-b9f6-d8a28a0633bf">

Find the folder icon on the top left and click "Open Script":

<img width="1440" alt="Screenshot 2024-06-05 at 1 31 07 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/16617791-c8e2-4e95-a687-a8dcf5325aff">

Navigate to the path where you put your script, in my case the Desktop, then go ahead and open it up:

<img width="1440" alt="Screenshot 2024-06-05 at 1 32 16 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/5e04a0d3-f52f-4536-bc73-7751e901286e">

Once it's open, we need to configure our execution policy to allow the server to run scripts, or else the rest of the process isn't going to work. To do this, in the prompt, type `Set-ExecutionPolicy Unrestricted` then hit enter. When you do, you will be prompted and you will click "Yes to All", then we are good to go:

<img width="1440" alt="Screenshot 2024-06-05 at 1 36 19 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/e226f5f9-26eb-41a4-90ca-602efd601bec">

Now, we will want to navigate to the folder where we stored the names, so in my case we type `cd C:\Users\a-wallimans\Desktop\AD_PS-master` and click enter. The `cd` command allows us to change directories:

<img width="1440" alt="Screenshot 2024-06-05 at 1 40 57 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/42780858-62d0-41aa-9ba4-7b735263bd08">

Now that we're in the right location, let's go ahead and click the arrow button to run!:

<img width="1440" alt="Screenshot 2024-06-05 at 1 42 40 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/1d66f5f1-95ec-4517-913d-0a0860147ef4">

As the script runs you should see the names actively being generated until eventually your screen should look something like this:

<img width="1440" alt="Screenshot 2024-06-05 at 1 49 15 PM" src="https://github.com/wallimans/Home-Lab/assets/170472167/fc232539-ea24-4110-814f-e34666730829">

To go ahead and verify everything worked as expected, let's open our "Active Directory Users and Computers" by entering and clicking it in our search bar, then we can expand our server and there we have it! We can see there was a new OU "_USERS" created with a bunch of individual users. Perfect! That concludes this part of the lab!
