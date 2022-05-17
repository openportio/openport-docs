Automating the installation
===========================

In this guide, replace the xxxxxxxxx by the token you see at the bottom of this page: [https://openport.io/user/keys](https://openport.io/user/keys)

Windows
-------

You can start the installer with the "/S" flag to do a silent installation:

.. code-block::

    openport_installer.exe /S

Then you can register your key:

.. code-block::

    openport register-key --token=xxxxxxxxx --name="unique name"


Start your share:

.. code-block::

    openport --restart-on-reboot 8000

Done!

Linux
-----

Install the .deb like you are used to:

.. code-block::

    sudo dpkg -i openport.deb

Register the key:

.. code-block::

    openport register-key --token=xxxxxxxxx --name=$HOSTNAME

Or you can use the MAC address:

.. code-block::

    mac_address=$(ifconfig eth0 | grep -o -E '([[:xdigit:]]{1,2}:){5}[[:xdigit:]]{1,2}')
    openport register-key --token=xxxxxxxxx --name=$mac_address

Start your share:

.. code-block::

    openport 22 --restart-on-reboot

Done!



