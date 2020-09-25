Alarms
======

By `Brett Burbidge`_

Using Alarms
------------

Alarms
^^^^^^
Using the network protocol Simple Network Management System (SNMP) the traffic system is constantly listening on the network to pre-configured network elements.  When these elements send a trap or notify message the alarm system listens for these specific messages and notifies technicians and others about the element status through a notification and on the UTA interface.

The notifications follow an escalation profile.  As each escalation response time expires another message will be sent until the notification is acknowledged.  For example the profile can be setup to notify a technician via mobile phone SMS message then, if the alarm is not acknowledged, 5 minutes later a phone call with the specific alarm and finally an email with a link to the alarm, the final alarm, in this example an email would be sent until the alarm is acknowledged. 

Here is the same example in list format.  The first notification is always sent, the following notifications will only be sent if the alarm has not been acknowledged. 
 #. SMS sent to technician with alarm specifics, wait 5 minutes.
 #. Phone call with specific alarm text, wait 10 minutes.
 #. Email sent with UTA link to alarm, sent every 15 minutes.

Auto Setup
^^^^^^^^^^

Some network elements send the same message number for all Critical, Major, and Minor alarms.  UTA can be configured to watch for these specific numbers and follow an escalation profile based on those specifics.  Many elements are already setup in UTA are ready to run upon installation.

