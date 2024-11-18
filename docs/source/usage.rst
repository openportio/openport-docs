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

Linking a key to your account
_____________________________

After creating an account, get your token from https://openport.io/user/keys

.. code-block::

    openport register-key <your-token>

After registration, your client and sessions will appear on the clients and sessions page.

Notes:

- If the client was already registered under a different account, it will be unregistered from that account
- Be careful when registering clients with sudo, in some cases this registers the wrong key because the $HOME variable is changed. (running openport as root is not recommended in any case).


Extra options
_____________

.. code-block::

      -d, --daemonize                     Start the app in the background.
          --exit-on-failure-timeout int   Specify in seconds if you want the app to exit if it cannot properly connect. (default -1)
      -h, --help                          Show help message
          --http-forward                  Request an http forward, so you can connect to port 80 on the server.
          --ip-link-protection string     Let users click a secret link before they can access this port. This overwrites the setting in your profile. choices=[True, False]
          --keep-alive int                The interval in between keep-alive messages in seconds. (default 120)
          --local-port int                The local port you want to expose. (default -1)
          --no-ssl                        Connect to the Openport servers without using SSL (only used if the --ws flag is set)
          --port int                      The local port you want to expose. (default -1)
          --proxy string                  Socks5 proxy to use. Format: socks5://user:pass@host:port
          --remote-port string            The server and port you want to expose locally. [openport.io:1234] (default "-1")
      -R, --restart-on-reboot             Restart this session when 'restart-sessions' is called (on boot for example).
      -v, --verbose                       Verbose logging
          --ws                            Use the websockets protocol instead of ssh.


Open-for-ip-link
----------------

To add an extra layer of security, all ports that are opened are by default protected by the open-for-ip-link feature.
This means that the user needs to click a link containing a token to access the forwarded port.

This can be disabled:

- by setting the --ip-link-protection flag to False when starting a session.
- by unchecking the "ip link protection" checkbox when editing a key on `the Keys page on your account <https://openport.io/user/keys>`_ .

Clicked links are valid for 24 hours.

You can check who clicked the links by using the API. https://openport.io/api/v2/openforiplinkclicks/
See the :ref:`documentation<api-open-for-ip-link-click>` for more information.



Volatile ports
______________

Each session opens a port on the openport servers. Because ports are not unlimited, it is possible that ports are reused.
We try to keep the same port for per key/local_port pair, but this is not guaranteed.

If you need a fixed address for an http server, you can use the "redirect_url" that are provided in the session api. For example: https://openport.io/r/pdDrqgF7/22 The links
are fixed per key. Following the link will redirect you to the correct server:port combination. You still need to click the open-for-ip-link if set.
You can set the local port in the last segment of the url.

See here for more information about the API click :ref:`here <api-sessions>`.


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
