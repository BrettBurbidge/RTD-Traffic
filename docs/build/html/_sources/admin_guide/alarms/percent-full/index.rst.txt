.. include:: /common/stub-variables.txt

Percent Full Alarm
------------------
 .. _percent-full-alarm:

Authors:  `Brett Burbidge`_

Introduction
^^^^^^^^^^^^

This example will demonstrate how to setup UTA to send an alarm when the Teleboss file gets too full.
 
Teleboss setup
^^^^^^^^^^^^^^

Ensure UTA & Teleboss Connections
"""""""""""""""""""""""""""""""""
Follow the steps in :ref:`Teleboss Connection <teleboss-connection>`  before you continue.

.. important:: If you followed the instructions above you should have a **Alarm Element** configured in your UTA system that is talking to the Teleboss.

Teleboss Settings
^^^^^^^^^^^^^^^^^

Percent Full Alarm Settings displays the menu for configuring alarms based on % fullness of the call record collection file.  The Teleboss can send an SNMP Trap when the file reaches a certain level.

.. note:: This alarm indicates that UCE is not pulling the call records out of the file.  Or that the SFTP connection is failing somewhere and the call records are not being moved to the SFTP server.

* Open Putty.exe and login to the Teleboss
* After logging into the Teleboss via SSH type 'setup' and press Enter.

.. image:: images/putty_login.jpg

* Select E (Alarm/Event Defintions)
* Select F (Percent Fill Alarm Settings)

.. image:: images/putty_percent_full.jpg

This is the menu that we will use to configure when to send UTA messages when the call records data file is getting too large. Lets go through each menu item.

**Alarm Enable** is an ON/OFF toggle to enable the percent full alarm. Default setting is OFF.

**Percent Full** Threshold set the percent full level at which the alarm will be triggered. Default setting is 80 percent.

**Alarm Actions** displays the Actions List, a menu where the action string for the event is configured. This field will be
empty [ ] if no actions have been configured, and will show [*SET*] if one or more actions have been configured.
Refer to Action List in the Features chapter for more information.

**Alarm Message** sets the text string to be delivered with the percentage full alarm. Default setting is DB Exceeds
Threshold. (Max length 111 chars)

**Alarm Class** sets the class for the alarm. When this option is selected, a list of the classes previously defined in the
Class Table is displayed, from which you can select one to be assigned to this percent full alarm.

**Trap Number** sets the number to be sent with any SNMP traps for this event. Default is 501, but trap number can
also be set in the range of 1000 – 1199 as needed.

For this exampe we will set the following:

A. Alarm Enabled: ON
B. Percent Full Threshold: [40]
C. Alarm Actions: trap(1)
D. Alarm Message: [DB Exceeds Threshold] leave as default, not used.
E. Alarm Class: [Info] leave default
F. Trap Number: 501

When finished your Teleboss Settings should look like this:

.. image:: images/putty_percent_full_complete.jpg

UTA Setup
^^^^^^^^^

This section we will setup a new **Alarm Monitor** to watch for the Percent Full SNMP Trap from the Teleboss.

#. Open UTA and login
#. Navigate to Admin > Alarm Elements

If you followed the Teleboss Connection tutorial here :ref:`Teleboss Connection <teleboss-connection>` you should already have an Alarm Element configured for this Teleboss.

#. Add a new Element Monitor by pressing the Monitors button on the Alarm Element 
#. Click New Monitor

.. image:: images/uta_new_alarm_monitor.gif

With the new Alarm Monitor open fill out the information for this monitor.  

* Name: Percent Full Alarm
* Alarm Key: the Trap Number you entered into the Teleboss.  501 from above.
* Alarm Key Value: leave this blank.
* Clear Alarm Key: leave this blank.
* Clear Alarm Key Value: leave this blank.
* Message: this is the alarm message that will be displayed when this alarm is fired.  For example this could be 'Teleboss CDR not being collected!'.
* Escalation: select the Escalation pattern from the dropdown list.
* Press the Save button to save the Alarm Monitor.

Example:

.. image:: images/uta_percent_full_monitor.jpg

Your Alarm Monitor is now ready to create alarms when the Teleboss file is above 40% full.

.. important:: In the real world 40% full alarms might not work.  If the Teleboss (via SFTP) or UCE can only push/pull the data from the file so fast, then this might be better set to a higher number.

Testing
^^^^^^^

To test this you need to fill the Teleboss file with data and watch for the Trap being sent to UTA.

To fill up FILE1 in the Teleboss fillow the instructions below. 

.. important:: These steps will mess up and possibly erase all data from all files in the Teleboss.  If that is okay, proceed.  If not then please proceed with caution and it would be good to know what you are doing...

Turn On IP Record Collection for General Server
"""""""""""""""""""""""""""""""""""""""""""""""

#. Open Putty, go to the Teleboss, login and type setup then press the Enter button.
#. Select A (Network Settings)
#. Select F (IP Record Collection Settings)

.. image:: images/putty_cdr_collection_menu.jpg

Cycle through the menu options by pressing the letter A. This should only take once press of A, but if you pass it press A until you see the following menu:

.. image:: images/putty_cdr_collection_menu_generic_server.jpg

Press the Enter button to leave this menu.

The Generic Server is now running against FILE1 on port 5000

Configure the Percent Full Alarm to 4%
""""""""""""""""""""""""""""""""""""""

Let's adjust the Percent Full number down to 4% so we can fill up the file with fake data.

Go back to Putty
#. Go back to the main setup menu and go into E (Alarm/Event Defintions)
#. Go into the F (Percnet Full Alarm Settings) menu
#. Enter B and set the value to 4 press the Enter button

Fill the File with Data
"""""""""""""""""""""""

We are going to use another instance of the Teleboss to fill the file1 with data.  Our goal is to fill it above 4% so we can test to make sure our settings are working.

#. Open another instance of Putty
#. Connect to the Teleboss but on port 5000. Like this:

.. image:: images/putty_connect_to_5000.gif 

#. When connected to port 5000 anything you type into the Putty window will be stored on FILE1.  
#. Go copy a huge document or a news site and paste it into the Putty window.  You paste by right clicking on the window.
#. Depending on how much data you are copying this might take 10-15 times before the file gets full.

**Check the size of the file**

#. To check the file size open the other Putty window.  Get to the main menu by pressing the Enter button mutiple times. 
#. Type the word status into command window and press the Enter button.  The status of the files will be shown.
#. If the file size is not past 4% keep pasting your data into the other Teleboss window.

.. image:: images/putty_paste_data_status.gif


I am pasting in the left window and checking the status in the right window.

Did it work?
""""""""""""

Navigate to UTA and go to Alarms > Traps.  You should see a Trap from the Teleboss with DB Exceeds Threshold text.

.. image:: images/uta_db_exceeds_threshold.jpg


Navigate to UTA and go to Alarms > Active Alarms.  You should see an Alarm generated from the Teleboss with 'The teleboss file1 exceeds....' text.

.. image:: images/uta_db_exceeds_threshold_alarm.jpg

