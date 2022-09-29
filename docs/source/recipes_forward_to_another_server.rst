Forward to another server
========================

If you have a device (let's call it machine A) that you cannot reach from your home (H), but you have another device (let's call it machine B) that does can reach machine A, you can forward your requests from H to B to A using these commands:

Let's say there is a webserver (port 80) running on A:

On B you run:

.. code-block::

    ssh -L 8000:<ip from A>:80 <user_on_B>@localhost
    openport 8000

That's it! Now you can access port 80 on A using the address the last command gave you.
