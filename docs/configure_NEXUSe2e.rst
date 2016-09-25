Configure NEXUSe2e for Justid
=============================

In the following paragraphes the configuration of NEXUSe2e is outlined. Use the same order to configure as in this document to get the fastest result


Change Password
---------------

Server Configuration -> User Maintenance -> Users
Click on “User, Administrator”

 [Save] + [Apply]



CA Certificates
---------------

CA Certificates -> “Add CA Certificate”

Download the necessary intermediate and root certificates. KPN is changing it’s intermediate certificate soon. Add both types, so that the change doesn’t have impact.

Staat der Nederlanden:
http://cert.pkioverheid.nl/RootCA-G2.cer
http://cert.pkioverheid.nl/DomOrganisatieCA-G2.cer

KPN old:
http://cert.pkioverheid.nl/KPN_Corporate_Market_CSP_Organisatie_CA_-_G2.cer

https://www.logius.nl/fileadmin/logius/ns/diensten/pkioverheid/certificaten/kpn/Jus4096-2_KPN_CM_CSP_Justitie_CA_-_G2.cer

KPN new:
http://cert.pkioverheid.nl/KPN_CSP_Organisatie_CA_-_G2.cer

If you have a Digidentity certificate:
http://cert.pkioverheid.nl/Digidentity_Organisatie_CA_-_G2.cer

https://www.logius.nl/fileadmin/logius/ns/diensten/pkioverheid/certificaten/digidentity/DigidentityServicesCA-G2.crt

For other PKI Overheid certificates check the site: https://cert.pkioverheid.nl/

Add the certificates and name them as:

Staat der Nederlanden Organisatie CA - G2
Staat der Nederlanden Root CA - G2
KPN Corporate Market CSP Justitie CA - G2
KPN Corporate Market CSP Organisatie CA - G2
KPN PKIoverheid Organisatie CA - G2
Digidentity Organisatie CA - G2
Digidentity Services CA - G2

The latter two can be different if you use a certificate from another party.


[Apply]


Frontend Pipeline
-----------------

Frontend Pipelines -> ebXML20OutboundPipeline
Select to configure “2. ebxmlHttpPacker”
Switch “Binary encoding” to [Binary]
Select “Include content disposition”

It is now:
Binary encoding         [Base64]
Include content disposition   [X]

[Save] + [Apply]



Backend Pipeline
----------------

Backend Pipeline -> FileLoadOutboundPipeline
Add “StaticEbmsHeaderPipelet”

1. FileLoad FileLoad    FileLoad
2. StaticEbmsHeaderPipelet Change specific header elements

Configure the StaticEbmsHeaderPipelet, set the values as:

Role From      AanleveraarTelecomgegevens
Role to     OntvangerTelecomgegevens
Service     AanleverenTelecomgegevens:1:0
Service Type   urn:ebv:services
Cut ContentId  [x]

[Save] + [Apply]


Create a new Backend Pipeline for Ping/Pong actions

[Add Pipeline]
Name:    PingPipeline
Direction      [Outbound]
Description Pipeline for Ping and Pong actions

Select [StaticEbmsHeaderPipelet] and click on [+] (Add)
Configure the StaticEbmsHeaderPipelet, set the values as:

Role From      AanleveraarTelecomgegevens
Role to     OntvangerTelecomgegevens
Service     urn:oasis:names:tc:ebxml-msg:service
Service Type   
Cut ContentId  [x]

[Save] + [Apply]



Server Identity
---------------

Server Configuration -> Server Identities -> Add Server Identity

Get elements from the CPA, <tns:PartyInfo> of your own entry
The PartyID must be the HRN of the own company (This is in the Netherlands the KvK number) padded with seroes in front and behind the HRN.
This number must also be part of the CN in the PKI Overheid certificate.

Partner Id  00000001234567890000    (get value from the CPA)
Partner Id Type   urn:osb:oin
Name     Providernaam         (get value from the CPA)

The rest can be left empty
[Save] + [Apply]


Add server certificate
----------------------

Server Configuration -> Certificates -> Certificate Staging -> Import Certificate
Choose the created .p12 file, with password fort his .p12 file

[Import]

Select certificate again and select “Promote to” -> [Promote Certificate]

[Apply]


Add connection to Server Identity
---------------------------------

Open the created Server Identity
Goto [Connections] -> click on [Add Connection]

Name     ebxml20-ssl
Connection URL https://jubes.provider.nl/digikoppeling/handler/ebxml20
(select your own url here; /handler/ebxml20 is typical for nexuse2e)

Description 
Certificate    “select the server certificate”
TRP      [ebxml-2.0-http]

Timeout:    30
Retries:    0

Leave the rest as default
[Save] + [Apply]



Collaboration Partner
---------------------

Collaboration Partners -> “Add Collaboration Partner”
 
Partner Id  0000000987654321001     (get value from the CPA)
Partner Id Type   urn:osb:oin
Name     CIOT           (get value from the CPA)

[Save] + [Apply]

Open the Collaboration Partner and select tab [Certificates] -> Add Certificate
a. Select the certificate file that was created with the certificates from the CPA (step “Ad 4.”). This is the certificate for jubes.minvenj.nl
b. Also add the certificate for the CN=jubes001.minjus.nl

In both cases the Certificate ID is the same as the Collaboration Partner ID. Now you must see 2 certificates.

[Apply]

Open the Collaboration Partner and select tab [Connections] -> Add Connection
Check values with the CPA.

Name  OntvangerTelecomgegevens_DC_T_Reliable_S_Transport_R_8PT3H
Connection URL       https://jubes.minvenj.nl/exchange/ciot
Description          
Certificate       000000001003214436001
TRP         ebxml-2.0-http
Timeout (sec)     10800
Message Interval (sec)  30
Secure
Reliable       X
Synchronous
Pick Up
Hold
Synchronous Timeout  0
Retries        8
Login Name  
Password

[Save]

Add another connection for the Ping/Pong actions, since these are not Reliable actions.

Name        OntvangerTelecomgegevens_DC_T_Besteffort_S__R_
Connection URL       https://jubes.minvenj.nl/exchange/ciot
Description          
Certificate       000000001003214436001
TRP         ebxml-2.0-http
Timeout (sec)     10800
Message Interval (sec)  30
Secure
Reliable       
Synchronous
Pick Up
Hold
Synchronous Timeout  0
Retries        0
Login Name  
Password

Note: Be sure to unselect the Reliable checkbox!

[Save] + [Apply]

NB: In the CPA are the values for the timeout and number of retries. For testing you can set the timeout on 60 seconds (and 0 retries) 



Choreography
------------

Choreographies -> Add Choreography

The Choreograpy ID is the CPA ID

Choreograhpy ID   ATG-1-0_00000001003214436001-00000001234567890000_0001         (get value from the CPA)
Description    GenericFile
         (important to name the Description “GenericFile”)

[Create] + [Apply]

Enter the Choreography and Add Action
FunctioneelAntwoorden
X  Valid Start Action
   Valid Termination Action
Backend Inbound Pipeline   [FileSaveInboundPipeline]
Backend Outbound Pipeline  [FileLoadOutboundPipeline]
Status Update Pipeline     [None]
Polling Required  
Document Type

Add another Action for “BestandenAanleveren” with the same values. Open again after creating it and select “FunctioneelAntwoorden” as Enabled Follow-Up Action

Create the Actions “OTAFunctioneelAntwoorden” and “OTABestandenAanleveren” in the same way. The actions are part of the CPA XML file. The actions “AntwoordBevraging” and “OTAAntwoordBevraging” don’t need to be added, since they are not in production yet.




For testing you can add a Ping and Pong action. Ping/Pong is an optional part of the ebMS standard. In Nexuse2e this must be defined as Actions.

Ping
X  Valid Start Action
   Valid Termination Action
Backend Inbound Pipeline   [FileSaveInboundPipeline]
Backend Outbound Pipeline  [PingPipeline]
Status Update Pipeline     [None]
Polling Required  
Document Type

Pong
   Valid Start Action
X  Valid Termination Action
Backend Inbound Pipeline   [FileSaveInboundPipeline]
Backend Outbound Pipeline  [PingPipeline]
Status Update Pipeline     [None]
Polling Required  
Document Type

Open Action Ping and select Pong as Enabled Follow-Up Actions.
[Update]

[Apply]

Select Partners
Goto the Choreography -> Participants -> Add Participant
Partner ID:    select CIOT
Local Partner ID: select your own created Server Identity
Local Certificate:   select your certificate
Connection: the created Connection

[Create] + [Apply]


Directory Scanner Service
-------------------------

Server Configuration -> Services -> Add Service

Name        CIOTBestandScanner
Component      [DirectoryScannerService]
Autostart      [X]
 
Scheduling Service   [SchedulingService]
Directory      /home/ciot/outbound  (example)
Backup Directory     /home/ciot/sent      (example)
Interval       30000
Choreography      ATG-1-0_00000001003214436001-00000001234567890000_0001      (CPAID from the CPA)
Action         BestandenAanleveren
Partner        00000001003214436001
(Value of Collaboration Partner / CIOT)
Extension      
Conversation      
Mapping Service   

[Save] and [Apply]





Also create a directory scanner for the OTA environment, for testing purposes.

Name        CIOTBestandScannerOTA
Component      [DirectoryScannerService]
Autostart      [X]
 
Scheduling Service   [SchedulingService]
Directory      /home/ciot/ota/outbound    (example)
Backup Directory     /home/ciot/ota/sent     (example)
Interval       30000
Choreography      ATG-1-0_00000001003214436001-00000001234567890000_0001      (CPAID from the CPA)
Action         OTABestandenAanleveren
Partner        00000001003214436001
(Value of Collaboration Partner / CIOT)
Extension      
Conversation      
Mapping Service   

[Save] and [Apply]

Create the directories on OS level.

When ready and tested the communication you can start the servers in the Services overview. First it is best to use the OTA directory scanner and put a test file in the OTA directory to test the directory scanner. If testing is successful and in cooperation with CIOT you can put a CIOT production file into the production directory and see if this is transferred right.



