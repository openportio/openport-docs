Restarting after a reboot
=========================


Windows
----
In windows, shares can be restarted of only the user that installed openport, and has checked the option to install the service during installation.

Linux & Mac
----
Openport will automatically restart the shares that the root user has started with --restart-on-reboot.
If you want to start the shares from other users too, you'll need to create a file at /etc/openport/users.conf and list the users here (one per line).