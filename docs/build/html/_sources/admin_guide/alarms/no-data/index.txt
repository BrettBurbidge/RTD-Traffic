.. include:: /common/stub-variables.txt

No Data Alarm
--------------
 .. _no-data-alarm:

Authors:  `Brett Burbidge`_

Introduction
^^^^^^^^^^^^

This example will demonstrate how to setup UTA to send an alarm when call records are not being fed into the Teleboss.  The steps below will configure the Teleboss to send an SNMP Trap to UTA that will trigger an alarm when no call records are coming in from the switch.

Teleboss setup
^^^^^^^^^^^^^^

Ensure UTA & Teleboss Connections
"""""""""""""""""""""""""""""""""
Follow the steps in :ref:`Teleboss Connection <teleboss-connection>`  before you continue.

.. important:: If you followed the instructions above you should have a **Alarm Element** configured in your UTA system that is talking to the Teleboss.

Teleboss Settings
^^^^^^^^^^^^^^^^^

No Data Alarms can be configured on the T850 to monitor data coming in via the serial ports, and take an alarm action
if a certain period of time passes with no data.

* Open Putty.exe and login to the Teleboss
* After logging into the Teleboss via SSH type 'setup' and press Enter.

.. image:: images/putty_login.jpg

* Select E (Alarm/Event Defintions)
* Select D (No-Data Alarm Settings)

.. image:: images/putty_no_data_menu.jpg

This is the menu that we will use to configure when to send UTA messages when no data is being fed to a file on the Teleboss. Lets go through each menu item.

No-Data n Alarm Settings allows you to configure two separate No-Data Alarms, each of which can be configured for
two different ranges of times with different time durations. The periods of time should be configured to match the
calling patterns of your business or organization. For example, if your normal business hours are M-F 8:00 to 5:00,
you will want to set lower time durations during those hours than you would “after hours” when call volumes are lighter
and the periods of time where there is "no data“ might be longer.

**Alarm Enable** is an ON/OFF toggle to enable the no-data monitor. Default setting is OFF.

**Alarm Actions** displays the Actions List, a menu where the action string for the event is configured. This field will be
empty [ ] if no actions have been configured, and will show [*SET*] if one or more actions have been configured.
Refer to Action List in the Features chapter for more information.

**Alarm Message** sets the text string to be delivered with this event’s alarms. Default setting is "No-Data Timeout n“.
(Max length 126 chars)

**Alarm Class** sets the class for the alarm. When this option is selected, a list of the classes previously defined in the
Class Table is displayed, from which you can select one to be assigned to this event.

**Trap Number** sets the number to be sent with any SNMP traps for this event. Default is 505, but trap number can
also be set in the range of 1000 – 1199 as needed.

**Schedule n Begin Time/End Time** sets the beginning and ending times (24 hr clock) for each of two ranges of time.

**Schedule n Duration** is the number of minutes (0-65535) the unit will wait without receiving data before alarming.

**Apply Alarm on Days** displays a menu where the seven days of the week are listed, and each can be toggled ON or
OFF to designate whether this particular No-Data alarm is active on that day. Default setting is ON for Monday thru
Friday, and OFF for Saturday and Sunday.

**Enable Ports** displays a menu where the installed serial ports are listed and each can be toggled ON or OFF to
designate whether this particular No-Data alarm is active on that port. Default setting is OFF for all ports.

**Add Exclusion/Delete Exclusion** allow you to add or delete specific dates when this No-Data Alarm should “take the
day off”. For example, Christmas is a day you might want to add here. Select Add Exclusion and type in 12/25. To
delete a date, you select Delete Exclusion and type in the date you want to remove. After an exclusion date is added
it appears in the brackets at the bottom of the menu. 15 dates can be entered to be excluded.

For this exampe we will set the following:

.. hint:: For real-life scenarios you will want to think about how this should be setup.  In the example we are using below it is setup to alarm if no data is received for 1 minute.  That might be a little aggressive in real life.

.. note:: Only one SNMP Trap will be sent when this alarm goes off.  For example if the Schedule 1 Duration is set to 1 minute, you will not get an alarm every minute.  It will only alarm once.


A. Alarm Enabled: ON
B. Alarm Actions: trap(1)
C. Alarm Message: Default is fine
D. Alarm Class: not used leave it as Info
E. Trap Number: 505 IMPORTANT this must be a unique number in the teleboss
F. Schedule 1 Begin Time: 00:00 (midnight)
G. Schedule 1 End Time: 13:00 (1pm)
H. Schedule 1 Duration: 1 (1 minute)
I. Schedule 2 Begin Time: 13:01 (1:01pm)
J. Schedule 2 End Time: 23:59 (11:59pm)
K. Schedule 2 Duration: 1 (1 minute)
L. Apply Days: default
M. Enable Ports: only enable the ports that you want to alarm on here
N. Add Exclusions: you would enter holidays in this field.  If you don't want alarms to be displayed on Christmas for example you could put 12/25
O. Delete Exclusion: used to delete exclusions from the list

When finished your Teleboss Settings should look like this:

.. image:: images/putty_no_data_menu_complete.jpg

UTA Setup
^^^^^^^^^

This section we will setup a new **Alarm Monitor** to watch for the No-Data SNMP Trap from the Teleboss.

#. Open UTA and login
#. Navigate to Admin > Alarm Elements

If you followed the Teleboss Connection tutorial here :ref:`Teleboss Connection <teleboss-connection>` you should already have an Alarm Element configured for this Teleboss.

#. Add a new Element Monitor by pressing the Monitors button on the Alarm Element 
#. Click New Monitor

.. image:: images/uta_new_alarm_monitor.gif

With the new Alarm Monitor open fill out the information for this monitor.  

* Name: No-Data Alarm 1.
* Alarm Key: the Trap Number you entered into the Teleboss.  505 from above.
* Alarm Key Value: leave this blank.
* Clear Alarm Key: leave this blank.
* Clear Alarm Key Value: leave this blank.
* Message: this is the alarm message that will be displayed when this alarm is fired.  For example this could be 'Teleboss File 1 is not receiving data'.
* Escalation: select the Escalation pattern from the dropdown list.
* Press the Save button to save the Alarm Monitor.

Example:

.. image:: images/uta_alarm_monitor.jpg

Your Alarm Monitor is now ready to create alarms when the Teleboss sends a 505 Trap message.

Testing
^^^^^^^

If you just let the Teleboss sit and not collect any data an alarm will be sent within about 1.5 minutes.  You can see this alarm in UTA.  

Navigate to Alarms > Traps to see if the trap made it to UTA:

.. image:: images/uta_no_data_trap.jpg

Navigate to Alarms > Active Alarms to see if the Alarm was generated:

.. image:: images/uta_no_data_alarm.jpg


Troubleshooting
^^^^^^^^^^^^^^^

You shold only be here if the above test failed.  Meaning that you were not able to see any traps or alarms in UTA.

System Log
""""""""""
Navigate to Admin > System Log and see if you can find any errors that happend about the time the Teleboss should have sent the no-data Trap.

Settings
"""""""
Go back through this page and make sure that all the settings between the Teleboss and UTA are setup properly.

Toggle the Setting to Try Again
"""""""""""""""""""""""""""""""
You can force the Teleboss to send another Trap by toggling the Alarm Enabled from ON to OFF.

* Putty into the Teleboss
* Type setup and press the Enter button
* Go to option E
* Go to option D
* Press A to disable the Alarm
* Wait 2-3 minutes
* Press A again to toggle the setting from OFF to ON.  
* Wait about 2 minutes and see if any traps were caught by UTA.

Finished
^^^^^^^^
At this point you should have UTA configured to send alarms when no data is being collected by the Teleboss.

.. note:: You can configure a different set of settings for File 2 by setting up No-Data 2 Alarm Settings and a new Alarm Monitor in UTA.  Just be sure to send different Trap Numbers for each Trap.
