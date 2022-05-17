Usage
=====

.. _installation:

Installation
------------

To use Openport, first download and install it:

.. code-block:: console
    $ wget https://openport.io/download/debian64/latest.deb
    $ sudo dpkg -i latest.deb

Sharing a port
--------------

Simply run

.. code-block:: console
    openport 22


Extra options
-------------

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
