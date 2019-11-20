Upgrade and Reinstallation
==========================

To upgrade from a previous 5.6.x version tot 5.7 it's best to do a complete fresh install. Probably with future major versions also it's better to reinstalll then upgrade.

It's also tried to save configuration, do a fresh install and then import the saved configuration. Efforts on this didn't get a good working system.

Remove an installation to start over
------------------------------------

Completely remove a previous install.

.. code-block:: text

	$ cd /usr/share/tomcat
	$ sudo systemctl stop tomcat
	$ sudo rm -f webapps/digikoppeling.war
	$ sudo rm -rf webapps/digikoppeling
	$ sudo rm -rf work/Catalina/localhost/digikoppeling
	$ sudo rm -rf NEXUSe2eDB

Remove logs if you like:

.. code-block:: text

	$ sudo find logs/ -type f -exec sudo rm {} \;
	$ sudo systemctl start tomcat

Now your Tomcat is running without NEXUSe2e.

Start again at the configuration description.