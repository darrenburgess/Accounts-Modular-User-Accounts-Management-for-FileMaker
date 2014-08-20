FileMaker-Accounts-Module
=========================

The Accounts module allows for management of user accounts in a FileMaker Pro in a single or multifile solution.

Assumptions
-----------
The module makes some assumptions about the security schema for multifile solutions.
* Each account that is managed by the module use the same privilege set in each file where accounts are to be managed.

Functions
---------
* Create
* Delete
* Enable/Disable
* Reset Password
* Change Privilege Set
* Validate Privilege Set
* User Welcome Email

Configuration Settings
----------------------
* Default Password
* Debugging On/Off
* Allow admin user to create passwords
* Require Password Change on first log in

Dependencies
------------
* None

Integration
-----------
Integration of the Accounts module should take less than an hour. There are multiple steps for a successful integration and they must be done in the proper order.

The FileMaker import.log file that is generated when importing objects such as scripts is an important measure of a successful integration. I recommend you integrate locally rather than on server.  The import.log file will conveniently appear in project directory when you import the scripts. If you are developing on a server, the import.log file should appear in the user documents directory.

###Quick steps for a failed integration
1. Don't read the instructions
2. Modify the scripts in the Private folder. As per Modular FileMaker guidelines, scripts in the private folder should not be edited.
3. Again, modifying scripts in the private folders is a POOR LIFE DECISION. All script modification happens to scripts in the public folders.

###Initial Set Up
1. I pity the fool that that don't make no back up!
2. Decide which file in your solution is the 'Master'. This will generally be the UI file in a separated solution.
3. Decide which file(s) in your solution will require account management. This will generally be the data file and can include the master file as well.
4. Optional - Satisfy dependancies by copying the custom functions to your solution files. These are the hash functions for passing and parsing multiple parameters. 

###Add the Accounts table and layout
1. Copy the module table 'Account' to your solution data file
	* If you already had an Account table, create or rename your fields so that the field names are the same.
	* The module does not care about your creation/modification account and timestamp fields or your ID. You do not need to rename them.
2. In the Master file create (or rename) a layout called 'Account'. 
	* This will be the utility layout used to create and delete account records.  You can rename it to conform to your naming conventions later.
	* Make sure it has all the fields in the Account table on it.
3. Copy and paste the GLOBAL table in the module file to your UI file.  
	* If you already have a GLOBAL table in UI, rename the primary TO for this table to GLOBAL, and just copy the fields found in the GLOBAL table.  You can rename the TO back to your conventions later.  Alternatively you can create another TO instance of your interface/global table and call it GLOBAL.

