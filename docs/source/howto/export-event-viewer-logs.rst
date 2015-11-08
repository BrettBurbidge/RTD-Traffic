.. include:: /common/stub-variables.txt

Export Event Viewer Logs
========================

Authors:  `Brett Burbidge`_

Introduction
^^^^^^^^^^^^
This document describes how to export the Event Monitor logs that are specific to the |product| product.  The Event Monitor is a log file built into windows.  |product| will write to this log if there are catastrophic events that prohibit the logs being written to the database or if there are windows based issues with the |product| service. This document describes how to export this data in an .evtx format.

Export the Event Viewer Logs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Follow the instructions below to export the UTA System Log.

#.	On the server where the UTA product is running open the Event Viewer by going to Start > Administrative Tools > Event Viewer
#.	Expand the Windows Logs folder
#.	Click on the Application folder
#.	In the Actions pane on the right click the Filter Current Log… button. A Filter Current Log window will appear. 
#.	There is a dropdown called Event sources:  In that drop down type Traffic and Alarms and click the OK button.
#.	The events in the main pane will be filtered.  
#.	If there are records in the main pane then follow the instructions below. If there is nothing in the main pane, then there are no events from Traffic and Alarms system in the event log.
#.	In the Actions pane on the right click the Save Filter Log File As… button. A Save As window will appear.
#.	In the File name enter UTA Event Viewer Log10202014.evtx.  Replace the 10202014 with todays date in this format DDMMYYYY.  That is todays date, todays month and todays year.




  
