FileMaker-Accounts-Module
=========================

The Accounts module allows for management of user accounts in a FileMaker Pro in a single or multifile solution.

Version 2.06 (version history is documented in the ReadMe script in the module file)

Do you want help integrating or customizing the module? Contact me to discuss your project: darrentburgess(at)gmail.com.

Source is on Github: https://github.com/darrenburgess/FileMaker-Accounts-Module

Development roadmap is in the Readme script in the module and in GitHub issues for this repo.

Assumptions
-----------
The module makes some assumptions about the security schema for multifile solutions.
* Each account that is managed by the module use the same privilege set in each file where accounts are to be managed.
* Each account that is managed by the module will be managed identically in each managed file.
* Privilege sets are named identically in each file where accounts are managed.
* You may have unmanaged files in your solution where you do not require accounts to be created, deleted, or updated. You will skip these files during the integration.

Functions
---------
* Create
* Delete
* Enable/Disable
* Reset Password
* Change Privilege Set
* Validate Privilege Set
* User Welcome Email (with option to send a solution launcher file)

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
Integration of the Accounts module should take less than an hour. There are multiple steps for a successful integration and they must be done in the proper order. If you get stuck, reach out for help: darrentburgess(at)gmail.com or twitter - @darrenburgess.

The FileMaker import.log file that is generated when importing objects such as scripts is an important measure of a successful integration. I recommend you integrate locally rather than on server.  The import.log file will conveniently appear in the project directory when you import the scripts. If you are developing on a server, the import.log file should appear in the user documents directory.

###Quick steps for a failed integration
1. Don't read the instructions.
2. Perform the steps out of order.
3. Don't ask for help.
4. Modify script parameters. (integration of the module requires no script parameter modification)
5. Modify the scripts in the Private folder. As per Modular FileMaker guidelines, scripts in the private folder should not be edited.

Again, modifying scripts in the private folders or any script parameters is a POOR LIFE DECISION. All script modification happens to scripts in the PUBLIC folders. Don't modify script parameters.

Important: Once you integrate the module, avoid modifying accounts in any of the managed files using the FileMaker security dialogs.  Doing so can create lack of parity between the actual accounts and the account records, causing errors in the module.  After integration try to only use the module to manage accounts.

###Initial Set Up
1. I pity the fool that that don't make no back up!
2. Decide which file in your solution is the 'master'. This will generally be the UI file in a separated solution.
3. Decide which file(s) in your solution will require account management. This will generally be the data file and can include the master file as well. These will be the 'managed' files.

###Tables and Layouts
1. Copy the module table 'Account' to your solution data file
	* If you already had an Account table, create or rename your fields so that the field names are the same. If there are any fields missing, copy them over to your existing table.
	* The module does not care about your creation/modification account and timestamp fields or your ID. You do not need to rename them.
2.	Add a table occurrence for the Account table to your master file if the master file is not your data file.
	* Add a external file reference to your data file if needed.
	* Name the table occurrence 'Account'
3. In the Master file create (or rename) a layout called 'Account'. 
	* Base the layout on the 'Account' table occurrence
	* This will be the utility layout used to create and delete account records.
	* You can rename it to conform to your naming conventions later.
4. Copy and paste the GLOBAL table in the module file to your master file.  
	* If you already have a globals table, copy and paste the fields in the module GLOBAL table to your table. This includes global fields, a constant, and a container field for the launcher file.  
	* If you already have a globals table, rename it to GLOBAL. You can rename the TO back to your conventions later.  
	* Alternatively, you can create another TO instance of your interface/global table and call it GLOBAL.
	* Create one record in the GLOBAL table (if there is not already).
	* Create a table occurrence for the GLOBAL table called 'System'. This is in addition to the GLOBAL table occurrence.
5. Create a layout for account management in the master file
	* Name the layout 'AccountManagement'
	* This will be the UI layout used to manage accounts.
	* The TO context for this layout should be System.
6. Create a layout based on the 'System' table occurrence in the master file.
7. In the master file, connect the System and Account table occurrences
	* Connect the TO for the System table to the TO for the Account table using the 'X' comparative operator connecting GLOBAL::Accounts_Constant to Accounts::id

###Value Lists and Privilege Sets
1. Create Value Lists
	* Create a Value List in master file: 'Accounts_PrivilegeSet' and add your privilege sets to the custom value list. Watch your spelling! 
	* Create a Value List in master file: 'Accounts_Boolean' with custom value of 1 and 0.
	* If you already have these value lists, rename what you have to the names provided above. You can restore your original names later.
2. Create Privilege Sets
	* For all files that will have managed accounts, create the privilege sets that the module will use. The module expects that for each managed file the privilege sets are the same. Check spelling, fool!
	* Create two 'dummy' privilege sets - 'PrivSet1', 'PrivSet2' - in all managed files. You will delete these later.

###Scripts
1. Copy Scripts: Master file only
	* Create a Modules script folder in the master file.
	* Clear or delete the import.log file before you import scripts. 
	* Master File Only: Copy the Accounts module folder and all scripts into your Modules script folder.
	* Important! Scripts should now be copied from your master file for all future script copy steps beyond this point.
	* Pro Tip: Open the import log in a text editor such as Sublime. Leave it open throughout the process. Sublime will refresh the log when changes are made by FileMaker. After you review each import, clear and save the blank log.
2. Error Check
	* This is a great time to do an error check of the import.log file.
	* Look at the log file and see if there are any lines that indicate something is 'missing'.
	* If there are any errors, try to figure out what you missed, then fix it, then delete and reimport the scripts.
	* Don't move on until you have resolved all errors.
3. Script Modification 1: Master file
	* Modify scripts in the 'Accounts: Public - All Files' folder in your master file.
	* Each public script will have instructions inside the script.
	* Instructions will involve copying a block of script steps for each privilege set in the solution.
	* Follow the instructions in each public script carefully, make sure to modify each line where called to do so.
	* Don't edit the Accounts: ErrorText or Accounts: AllowAbort scripts.  They are in public, but do not need to be edited. 
4. Copy Public - All Files scripts 
	* Create a Modules script folder in each managed file.
	* Creates an Accounts script subfolder in Modules script folder in each managed file.
	* Copy the 'Accounts: Public - All Files' folder to the Modules/Accounts script folder in each managed file.
	* Check the import.log file for any new errors.
5. Copy Private Scripts: Managed file(s)
	* From the Master file: Copy 'Accounts: Private All Files' script folder and all scripts in the folder to the Modules/Accounts folder in each managed file.
	* Check the import.log file for any new errors.
6. Script Modification 2: Master file
	* Modify scripts in the 'Accounts: Public - Master File Only' folder
	* Each of the scripts with 'SubScript' in the script name will need to call their respective scripts in each managed file.
	* Follow the instructions carefully, changing the script steps as indicated in the script comments.
	* If the master file is also a managed file, then the first script block is already configured for the master file. No need to change it.
7. Script Modification 3: Accounts: ReturnAccountSettings
	* Modify the script Accounts: ReturnAccountSettings in the master file.
	* Specify a default password.
	* Modify other settings as desired.

###User Interface and Testing
1. Add User Interface Elements
	* Copy and paste all objects in the layout body of the layout 'AccountManagement' to the 'AccountManagement' layout in the master file.
	* Make sure there are no broken field references on the layout.
	* Add your launcher file to the launcher container field.
	* If you already had user interface for showing accounts in a portal, then you will have to modify the buttons to run the Accounts: Account Function script.  Check the module buttons for the proper syntax for the script parameters on the buttons. Don't modify the script parameters, just copy and paste to the appropriate button.
2. Testing and Wrapup
	* Test each function: Create/Delete/Status/Reset/Change Privilege/Email
	* Test the function and then check each managed file to verify that the function executed properly.
	* If testing is successful, revert the names of each object you renamed for the integration.
	* If there were already accounts in each of your managed files, they should be removed and recreated.
	* Delete the dummy privilege sets PrivSet1 and PrivSet2 from each of the managed files.

Nicely done!

###Contact me if you have any questions:
	* darrentburgess(at)gmail.com
	* @darrenburgess on twitter.





