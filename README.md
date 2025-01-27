# WriteUps
Some of my Hack the Box writeups

# CPToDrawIO.py
A python3 script to take an exported Checkpoint config as input and output a csv in the format required by Draw.IO to auto-generate a logical network diagram based on rules in Checkpoint. 

# bulknet.py
A Python script that uses the netmiko library to send bulk backup, show or configuration commands to network devices. 
Currently accepts -h for basic usage and [-f logfile_name] to duplicate the output to a log file.

SSH access to each device is required from the device that is running the script. Structures for network devices are hard-coded from Line 34-117.

To make this work on a different network modify the device and string lists from line 119-135 to match the devices you want to connect to. The strings list is only for printing out the device name to stdout when it's being connected to and commands are being passed. The device lists are a list of the structs defined from 34-117. TODO: modify hard-coded string and device lists to read from csv.

The last required change is to the device_choice function from line 277-323. Set the devices variable to one of the device struct lists defined above, as well as the corresponding string to device_string. The if choice >= 5 at line 321 is essential because firewall configuration and show commands require a different function. So make sure lists of firewalls and switches/routers are kept in separate lists and that lists of firewalls are in choice>=5, or change the logic to something better. 

Those are the only modifications that are required. Tested to work on 
Switch Version 15.0(2)SE10a C2960-LANBASE SW Image C2960-LANBASEK9-M
Switch Version 12.2(55)SE5 C3560-IPSERVICESK9-M
Switch Version 16.3.5b CAT3K_CAA-UNIVERSALK9-M
Router Version 15.1(4)M6 C2800-ADVENTERPRISEK9-M
Router Version 15.2(4)M3 C2900-UNIVERSALK9-M

# ImportUsersToAD.ps1
A Powershell script to add users to Active Directory while creating the necessary groups and OUs. Creation of Groups and OUs requires user confirmation. Creation of users does not require confirmation. This script checks if the user, group or OU exist before creating any of them and continues if they do, or offers to create them if they don't. An existing group or OU elicits no message but a message will be printed for an existing User. Existing Users will not be modified in any way.

A few variables are hardcoded, such as domain, user container/path, and credentials (line 8 and lines 163-169).

Use a CSV file with the headings used in 'NETE2980 - Employee Names.csv' where Group1 is the Organizational Unit, Group2 is a primary group and Group3 is an optional secondary group. Additional AD user information can be entered at line 97, like mail addresses etc.. For example, right now it will put all the new users in Company "Dominion Greenhouses". The format of the login name can be modified at lines 92-94. Currently 91 truncates the SamAccountName to 20 chars, and removes any spaces in first or last name and joins firstname+lastname to create the User Principal Name.

The provided file is in .xlsx format so one option is to convert it using Excel. CSV files should be checked for non-ascii characters prior to running ImportUserstoAD, one option is to use findnonascii.py.

OUs are created in a heirarchical structure, and security groups are created for every new OU. This allows OUs to be placed on departments within locations, such as separate OUs for Accounting at Headquarters and Manufacturing, while still having a security group that applies to all members of Accounting.

# findnonascii.py
The ImportUserstoAD scripts do not like non-ascii characters. This script will find lines with non-ascii characters and print them out with a line number so they can be manually changed before running ImportUserstoAD.ps1

# getpinging.ps1
Script to continuously ping a list of IPv4 and IPv6 addresses to test distribution router HSRP failover.
