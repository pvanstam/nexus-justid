Prerequisites
=============


System Requirements
-------------------

`From the NEXUSe2e website <https://www.nexuse2e.org/NEXUSe2e/Documentation/Installation/System-Requirements.html>`__:

* Linux or Windows
* Dual core CPU
* 4GB RAM
* About 10GB of free HDD space

Software

* `Java JDK 8 64bit * <https://www.oracle.com/technetwork/java/javase/downloads/index.html>`__
* `Apache Tomcat 9 <http://tomcat.apache.org/>`__
* `Java Cryptography Extension (JCE) Unlimited <https://www.oracle.com/technetwork/java/javase/documentation/jdk11-readme-5097204.html#jce>`__
* `Hibernate 4 supported database <https://developer.jboss.org/wiki/SupportedDatabases2>`__

\* Remark: If you are running Linux do not use openJDK.

From practise the following remarks

* Running on 2GB of memory is working for a dedicated VPS, with small files.
* Files are loaded in memory. For transferring larger files, more memory is needed. This can be trial and error.
* Running NEXUSe2e with OpenJDK is confirmed to be working.
* JCE extension is absolutely necessary. This is already part of OpenJDK or Java JDK 11 and later.