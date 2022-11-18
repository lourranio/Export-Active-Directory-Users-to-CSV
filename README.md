# Export-Active-Directory-Users-to-CSV
 Powershell: Export Active Directory Users to CSV
 
Exporting Users from Active Directory is a really simple task, even if you’re not very familiar with PowerShell.

As long as you have an account with sufficient permissions to read from Active Directory you’re good to go.

Lets step through a few examples below of the most common scenarios to export ad users to csv (and one method that doesn’t involve PowerShell for people who prefer prefer a GUI or just dislike PowerShell).

Consideration: Before you export your accounts, I recommend downloading the FREE Admin Bundle for Active Directory to clean up all your inactive user and computer accounts.

## How to Export Users from Active Directory

The first thing you to do is open a PowerShell session either locally on a machine running the AD DS role (like a Domain Controller) or install the Remote Server Admin Tools (RSAT) so that the Active Directory module is available.

Related: How to Install Remote Server Administration Tools (RSAT) for Windows 10

When it comes to exporting users you have a lot of options for the information you want exported. User accounts have a lot of associated attributes (which you can see if you go to Extension -> Attributes in Active Directory Admin Center).

Another way to see the attributes you have available to export is to run the following command within your PowerShell window:

```get-aduser rsanchez -properties *```

Now that you know what you’re after, lets look at some examples!


## Export All AD Users by Name to CSV

```Get-ADUser -Filter * -Properties * | Select-Object name | export-csv -path c:\temp\userexport.csv```

This command will export all of the user accounts in your domain to a CSV by their name. What this means is that the CSV file will contain a single column list of every account’s First, Middle, and Last name.

The ‘export-csv -path c:\temp\userexport.csv’ after the pipe (the | character) is what exports the data to CSV. If you drop that section of the command it’ll simply print the results to your PowerShell window.


## Export All AD Users by Name and LastLogonDate

If you want a list of every account’s name and the last date the account authenticated with the domain then you can run the command:

```Get-ADUser -Filter * -Properties * | Select-Object name, LastLogonDate | export-csv -path c:\temp\userexport.csv```

This will export all accounts in your domain to a CSV sheet with the First, Middle, and Last name in the first column and LastLogonDate in the second column.

If you want to export more attributes you just need to add them the Select-Object section of the command separated by a comma. For example:


## Export All AD Users by Multiple Attributes

```Get-ADUser -Filter * -Properties * | Select-Object name, lastlogondate, department | export-csv -path c:\temp\userexport.csv```


## Export All AD Users Email Addresses

```Get-ADUser -Filter * -Properties * | Select-Object mail | export-csv -path c:\temp\userexport.csv```


## Export All AD Users from Specific OU (Organizational Unit)

Before you run this command you need to find the distinguishedName attribute of your OU. You find this by opening the properties of the OU in Active Directory Admin Center and going to Extensions -> Attribute Editor.

```Get-ADUser -Filter * -SearchBase "OU=Research,OU=Users,DC=ad,DC=npgdom,DC=com"
-Properties * | Select-Object name | export-csv -path c:\temp\userexport.csv```

At this point I’m betting you’re getting the gist of it. Like I said, pretty easy!

Need to import users?: Solarwinds has a free and dead simple user import tool available as part of their Admin Bundle for Active Directory that I recommend taking a poke at.
