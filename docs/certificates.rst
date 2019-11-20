Certificates
============

Certificates are necessary for the secure communication channels.

TLS is used for both the https channel and the ebXML message exchange itself.

Configure certificates in the webserver
---------------------------------------

In this documentation `NGINX <https://www.nginx.com/>`__ or `Apache <https://www.apache.org/>`__ is used as a frontend for HTTPS to Tomcat.

For HTTPS the PKI Overheid certitifcate can be used. The Common Name (CN) doesn't need to be the same as the web server's name. The CA certificates can be stored together with the server's certificate in a chained certificate file.