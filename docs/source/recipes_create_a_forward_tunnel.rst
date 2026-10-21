Creating a forward tunnel
=========================

Consider next scenario:

I have a pc at home, that I want to contact from my work.
Both locations are firewalled and do not allow outgoing connections to ports other than 80 (http) and 443 (https).

Connecting by device name
_________________________

On my home pc: I use the commands

.. code-block::

    openport register xxxxxxxx --name home-pc # From your https://openport.io/user/keys page
    openport 22 --restart-on-reboot

On my work pc: I register to the same account and connect to the device by its name:

.. code-block::

    openport register xxxxxxxx
    openport ssh myhomeuser@home-pc

This opens a tunnel to port 22 of ``home-pc`` and starts ssh through it, in one go.
Extra ssh options and a remote command go where ssh expects them:

.. code-block::

    openport ssh -i ~/.ssh/id_ed25519 myhomeuser@home-pc uptime

For other ports and protocols, ``connect`` opens the tunnel without assuming ssh:

.. code-block::

    openport connect home-pc 3306

Which will give me the output:

.. code-block::

    INFO - You are now connected. You can access the remote pc's port 3306 on localhost:58553

``connect`` can also run a command of your own once the tunnel is up. ``$port`` is
replaced by the local port of the tunnel; single-quote the command so your own shell
does not expand it first. The tunnel lives as long as the command and exits with its
exit code:

.. code-block::

    openport connect home-pc 5432 exec 'psql -h localhost -p $port -U postgres'

Notes:

- Both machines must be registered to the same account, and the device name must be
  unique within your account. You can rename devices on https://openport.io/user/keys .
- If the connection fails with "Please pass request_port ...", the server does not
  support connecting by name yet; use the port-based forward below.

Connecting by port
__________________

The name-based commands above resolve to a forward tunnel by port, which you can also
set up yourself. On my home pc:

.. code-block::

    openport register xxxxxxxx # From your https://openport.io/user/keys page
    openport 22 --restart-on-reboot

Which gives me the output:

.. code-block::

    INFO - Now forwarding remote port openport.io:4104 to localhost:22.

On my work pc: I use the command

.. code-block::

    openport register xxxxxxxx # From your https://openport.io/user/keys page
    openport forward --remote-port 4104 -R

Which will give me the output:

.. code-block::

    INFO - You are now connected. You can access the remote pc's port 22 on localhost:58553

Now, if I want to connect to my home pc, I can use the command

.. code-block::

    ssh myhomeuser@localhost -p 58553

``forward`` accepts the same ``exec`` tail as ``connect``:

.. code-block::

    openport forward --remote-port 4104 exec 'ssh myhomeuser@localhost -p $port'
