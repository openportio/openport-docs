Usage
=====

.. _installation:

Installation
____________

To use Openport, first download and install it:

.. code-block::

    $ wget https://openport.io/download/debian64/latest.deb
    $ sudo dpkg -i latest.deb

Other platforms available from our `Github <https://github.com/openportio/openport-go/releases>`_ page.

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
      -d, --daemonize                     Start the app in the background.
          --exit-on-failure-timeout int   Specify in seconds if you want the app to exit if it cannot properly connect. (default -1)
          --http-forward                  Request an http forward, so you can connect to port 80 on the server.
          --ip-link-protection string     Let users click a secret link before they can access this port. This overwrites the setting in your profile. choices=[True, False]
          --keep-alive int                The interval in between keep-alive messages in seconds. (default 120)
          --port int                      The local port you want to expose. (default -1)
          --proxy string                  Socks5 proxy to use. Format: socks5://user:pass@host:port
          --remote-port string            The remote port on the server. [openport.io:1234] (default "-1")
      -R, --restart-on-reboot             Restart this session when 'restart-sessions' is called (on boot for example).
      -v, --verbose                       Verbose logging

Files used
__________

All log files and the local sqlite database are stored under the .openport folder in the user home (~/.openport).


Restarting after a reboot: Windows
__________________________________
In windows, only the user that installed openport shares can be restarted on reboot, and the option to install the service during installation must be checked.

Restarting after a reboot: Linux & MacOS
________________________________________
Openport will automatically restart the shares that the root user has started with --restart-on-reboot.
If you want to start the shares from other users too, you'll need to create a file at /etc/openport/users.conf and list the users here (one per line).
