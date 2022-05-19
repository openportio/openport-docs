Usage
=====

.. _installation:

Installation
____________

To use Openport, first download and install it:

.. code-block::

    $ wget https://openport.io/download/debian64/latest.deb
    $ sudo dpkg -i latest.deb

Sharing a port
______________

Examples:

.. code-block::

    openport 22
    openport 8080 --http-forward
    openport 3389 --restart-on-reboot


Extra options
_____________

.. code-block::

    Usage: openport (<port> | forward | list | kill | kill-all | register | help )
    Default: openport <port> [arguments]
      -d, --daemonize                   Start the app in the background.
          --http-forward                Request an http forward, so you can connect to port 80 on the server.
          --ip-link-protection string   Lets users click a secret link before they can access this port. This overwrites the setting in your profile. choices=[True, False]
          --keep-alive int              The interval in between keep-alive messages in seconds. (default 120)
          --local-port int              The local port you want to expose. (default -1)
          --port int                    The local port you want to expose. (default -1)
          --proxy string                Socks5 proxy to use. Format: socks5://user:pass@host:port
          --remote-port string          The remote port on the server. [openport.io:1234] (default "-1")
      -R, --restart-on-reboot           Restart this session when 'restart-sessions' is called (on boot for example).
      -v, --verbose                     Verbose logging

Files used
__________

All log files and local databases are stored under the .openport folder in the user home (~/.openport).


Restarting after a reboot: Windows
__________________________________
In windows, shares can be restarted of only the user that installed openport, and has checked the option to install the service during installation.

Restarting after a reboot: Linux & MacOS
________________________________________
Openport will automatically restart the shares that the root user has started with --restart-on-reboot.
If you want to start the shares from other users too, you'll need to create a file at /etc/openport/users.conf and list the users here (one per line).
