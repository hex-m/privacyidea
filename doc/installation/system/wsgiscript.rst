.. _wsgiscript:

The WSGI Script
===============

Apache2 and Nginx are using a WSGI script to start the application.

This script is usually located at ``/etc/privacyidea/privacyideaapp.py`` or
``/etc/privacyidea/privacyideaapp.wsgi`` and has the following contents::

   import sys
   sys.stdout = sys.stderr
   from privacyidea.app import create_app
   # Now we can select the config file:
   application = create_app(config_name="production",
                            config_file="/etc/privacyidea/pi.cfg")

In the ``create_app``-call you can also select another config file.

.. note:: This way you can run several instances of privacyIDEA in one
   Apache2 server by defining several WSGIScriptAlias definitions pointing to
   different wsgi-scripts, that again reference different config files with
   different database definitions.

Running Apache instances
------------------------

.. index:: instances

To run further Apache instances add additional lines in your Apache config::

    WSGIScriptAlias /instance1 /etc/privacyidea1/privacyideaapp.wsgi
    WSGIScriptAlias /instance2 /etc/privacyidea2/privacyideaapp.wsgi
    WSGIScriptAlias /instance3 /etc/privacyidea3/privacyideaapp.wsgi
    WSGIScriptAlias /instance4 /etc/privacyidea4/privacyideaapp.wsgi

It is a good idea to create a subdirectory in */etc* for each instance.
Each wsgi script needs to point to the corresponding config file *pi.cfg*.

Each config file can define its own

 * database
 * encryption key
 * signing key
 * ...

To create the new database you need the command *pi-manage*. The command
*pi-manage* reads the configuration from */etc/privacyidea/pi.cfg*.

If you want to use another instance with another config file, you need to set
an environment variable and create the database like this::

   PRIVACYIDEA_CONFIGFILE=/etc/privacyidea3/pi.cfg pi-manage create_tables

This way you can use *pi-manage* for each instance.
