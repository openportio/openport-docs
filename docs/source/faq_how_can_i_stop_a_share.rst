Stopping a session
==================

You can kill shares via the manager using:

.. code-block::
    openport manager --list
    openport manager --kill <local port>

On linux and mac:
-----------------
Ctrl-c or kill -2 will stop the share and the share will not restart on reboot.

Ctrl-z or kill -9 will stop the share, but the share will restart if the --restart-on-reboot flag was passed.


On windows:
-----------
Ctrl-c will stop the share and the share will not restart on reboot.

Ctrl-z or stopping via task manager will stop the share, but the share will restart if the --restart-on-reboot flag was passed.
