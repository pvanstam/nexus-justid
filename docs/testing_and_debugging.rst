Testing and debugging
=====================

Connectivity
------------

1. Test reachability port 443

On command line you can enter something like:
$ telnet jubes.minvenj.nl 443

Response is like:
Trying 159.46.196.104...
Connected to jubes.minvenj.nl.
Escape character is '^]'.

Then you have a successful connection.
Enter {Xtrl]+]  and then quit[Enter]

2. Test ebms connectivity

Create a CA-bundle of the PKIoverheid certificates to validate the Justid certificate. In a step before the CA certificates were retrieved. Put them into a ca bundle file.

$ openssl x509 -inform DER -in staatdernederlandenrootca-g2.crt \
   >pkioverheid-bundle.pem
$ openssl x509 -inform DER -in staatdernederlandenorganisatieca-g2.crt \
   >>pkioverheid-bundle.pem
$ openssl x509 -inform DER -in Jus4096-2_KPN_CM_CSP_Justitie_CA_-_G2.cer \
   >>pkioverheid-bundle.pem
$ openssl x509 -inform DER -in KPN_Corporate_Market_CSP_Organisatie_CA_-_G2.crt \
   >>pkioverheid-bundle.pem

Try to reach the ebms interface of justid, with ‘curl’ command:

# curl -v --key ../private/sbr.xxx.nl.key --cert ./sbr.xxx.nl.cert \
   --cacert pkioverheid-bundle.pem https://jubes.minvenj.nl/exchange/ciot

# curl  --key ../private/sbr.alphaco.nl.key --cert ./sbr.alphaco.nl.cert \
  --cacert pkioverheid-bundle.pem -I https://jubes.minvenj.nl/exchange/ciot

If the connection is successful – without SSL errors – then a 400 return code will be presented, because the request is not an ebMS SOAP request:

HTTP/1.1 400 Bad Request



ebms Ping/Pong
--------------

For Ping/Pong you first need to select the BestEffort connection.

Goto Choreographies -> select the choreography -> Partitipants -> CIOT
Select the Besteffor connection + [Update] + [Apply]

Then: with Tools -> Message Submission
Select the Choreography
Select the Action [Ping]

Then click on [Execute]

In Reporting -> Transaction Reporting the log of the message submission should be available. On receiving the Pong you will see a second log line. The status should be Completed (with 2 log lines).

Set the Connection of the Participants CIOT Connection back to Reiliable.
[Update] + [Apply]


Message Submission
------------------

With Message Submission you can do testing by sending a file.
Tools -> Message Submission

You can send any file. Normally you should the OTABestandAanleveren action for this.

Debugging with local delivery can be done with the following method.

Add connection to the Collaboration Partner

Collaboration Partners -> CIOT -> [Connections] -> Add Connection
Name:       Localhost-testconnection
Connection URL:   http://localhost:8888/
Certificate:      [ - none - ]
TRP:        [ebxml-2.0-http]
Retries:    1

[Save] + [Apply]

For test change the connection in the Participant in the Choreography
Choreography -> 'your CPA ID' -> Participants -> select the partner ID
Connection: select the Localhost-testconnection

Don't forget to modify back to the CIOT connection after testing.

[Update] + [Apply]

On the server itself, start 'nc' (netcat) with the port 8888 to see what Nexuse2e is sending:

$ sudo nc -l 8888

Then start a Message Submission in Tools. Select a binary file to send (!).
The nc gives the ebMS message as output on the screen.
There is of course no feedback, so Nexuse2e message submission will fail, but it gives an indication of the output of nexuse2e

Go to Reporting -> Transaction reporting to see results.



