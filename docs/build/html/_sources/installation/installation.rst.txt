.. include:: /common/stub-variables.txt

Installing |product|
====================

Authors:  `Brett Burbidge`_

How to install |product|
^^^^^^^^^^^^^^^^^^^^^^^^

Setup
^^^^^
Download the installer by going to the FTP site to go #Down/UTA - Unique Traffic & Alarms/Release and grab a copy of the version with the highest release number (UTA[version].zip). 

As of this writing the release version is UTA1.0.7.2.zip. 

Database Install
^^^^^^^^^^^^^^^^
To prepare the UTA database follow the instructions below. 
 #. Create a database in SQL Server called UTA.  Make sure that a user has access to this database. For example, use the same user as Cairs, CairsUser. 
 #. Unzip the install files on a computer with access to the database sever or on the database server itself. 
 #. Find the bat file called Update_UpdateDatabaseOnly.bat and edit the database connection string in that file. 
 #. Save and Close the file then run it with PowerShell from a computer that has access to the database.  You will not need to run this as Administrator.
 #. After running you will se a message that says Database update finished! Database Update Finished Successfully!
 #. Database Install done!

Web Install
^^^^^^^^^^^
To install the web portion of UTA follow the instructions below. 
 #. Ensure IIS is installed and functional.  Use the same IIS components as Cairs.net. 
 #. Unzip the install files on the Web Server. 
 #. Create a new folder called UTA either on the C or E drive. 
 #. Copy the folder contents of Application/Web and paste the folders and files to the new UTA folder. 
 #. Open IIS expand ServerName > Sites > Default Web Site
 #. Right click Default Web Site and select Add Application
 #. In the Add Application dialog box follow these steps:

    #. Alias: UTA
    #. Application Pool: DefaultAppPool
    #. Physical Path: The path to the UTA folder created above

 #. In IIS Select the newly created UTA directory.

Set the database connection string by opening Connection Strings under ASP.NET 
Open the sqlServer connection string and point it the database you created in a previous step

Forms authentication (Username and password) 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
*I Suggest using Forms if this is the first time you set this up. It makes it easier to test everything.*

Open the Authentication icon under IIS
 #. Enable Anonymous Authentication
 #. Enable Forms Authentication
 #. Disable all other options

Go back to the main IIS Window and select Application Settings
Set AUTHENTICATION_MODE to Forms

Windows Authentication (domain/user)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open the Authentication icon under IIS
 #. Enable Windows Authentication
 #. Disable all other options

Go back to the main IIS Window and select Application Settings
Set AUTHENTICATION_MODE to Windows

For Certificate Authentication (CAC)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Open the Authentication icon under IIS
 #. Enable Anonymous Authentication
 #. Disable all other options

Go back to the main IIS Window and select Application Settings
Set AUTHENTICATION_MODE to Certificate

Open SSL Settings
 #. Require SSL
 #. Under Client Certificates set Require
 #. Apply the settings by pressing Apply on the right

.. note:: To test everything works go to a browser that has access to this web server or open the browser on this server and go to http://localhost/uta or http://computername/uta
 The default username and password is: admin and b for the password. 

Service Install
^^^^^^^^^^^^^^^
To install the UTA service follow the instructions below

This can be installed on the web or app server as long as the server has access to the database.  If you are installing this for Alarms other devices on the network will need to communicate over SNMP to this server.  Just keep that in mind moving forward. 
 #. Unzip the install files on the server. 
 #. In the Application folder find the Installers folder and run the installer called UTAServiceInstaller.msi
 #. Go through the prompts and make sure to check Install for Everyone.
 #. Open Windows Explorer and navigate to the location of the UTA service. Normally this is in C:\ Program Files(x86)\Unique Communications\UTA\
 #. Find the named TrafficService.exe.config and open it for editing in notepad. 
 #. On line 6 of this file is the connection string. Make sure the connection string is correct.  One way to do this is to copy the connection string out of the UTA Web.config file on the Web Server. 
 #. If the customer wants to report on CAIRS Call Records enter the CAIRS_CONNECTIONSTRING value for the Cairs database. 
 #. Go to Service Manager and start the service. 

Test Service Installation
^^^^^^^^^^^^^^^^^^^^^^^^^
Navigate to http://localhost/uta or http://computername/uta enter admin for the password and b for the password. 
 #. Open the Admin/System Log menu item
 #. You will see data in the table pertaining to starting services.  If you don’t please review the steps in the previous section. 

Enable |product| to Aggregate and report on Cairs Call Records
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to enable this functionality the Cairs connection string setting must be set in the web.config of |product| website and also in the app.config of the |product| Service. 

**Web Configuration**
 #. Open IIS, select Default Web Site, Select UTA, double click on Application Settings.
 #. Edit CAIRS_CONNECTIONSTRING set Value to the cairs database connection string. 

**Service Configuration**
 #. Go to the file location (1.0.x.x\Application\Service)
 #. Open the TrafficService.exe.config file with notepad
 #. Edit CAIRS_CONNECTIONSTRING set the value to the cairs database connection string. 

The service will now aggregate the CDR collected by Cairs from the CallRecord table. Reports for each Cairs Switch will be available from the main Traffic window in |product|.