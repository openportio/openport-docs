Creating a forward tunnel
=========================

Consider next scenario:

I have a pc at home, that I want to contact from my work.
Both locations are firewalled and do not allow outgoing connections to ports other than 80 (http) and 443 (https).

On my home pc: I use the commands

.. code-block::

    openport register-key xxxxxxxx # From your https://openport.io/user/keys page
    openport 22 --restart-on-reboot

Which gives me the output:

.. code-block::

    INFO - Now forwarding remote port openport.io:4104 to localhost:22.

On my work pc: I use the command

.. code-block::

    openport register-key xxxxxxxx # From your https://openport.io/user/keys page
    openport forward --remote-port 4104 -R

Which will give me the output:

.. code-block::

    INFO - You are now connected. You can access the remote pc's port 22 on localhost:58553

Now, if I want to connect to my home pc, I can use the command

.. code-block::

    ssh myhomeuser@localhost -p 58553