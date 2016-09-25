Certificates
============

Certificates are necessary for the secure communication channels.

TLS is used for both the https channel and the ebXML message exchange itself.

Configure certificates in the webserver
---------------------------------------

In this documentation nginx (or apache) is used as a frontend voor https to Tomcat. Nginx needs to use a server key and certificate and CA certificates.

For HTTPS the PKI Overheid certitifcate can be used. The Common Name (CN) doesn't need to be the same as the web server's name. The CA certificates can be stored together with the server's certificate in a chained certificate file.




