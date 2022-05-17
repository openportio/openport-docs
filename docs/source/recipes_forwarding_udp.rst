Forwarding UDP
==============

Openport uses reverse-ssh behind the scenes, so UDP forwarding is not (yet) supported out of the box. However, with some scripting you can achieve UDP forwarding.

This guide uses the command "socat" which is included in most Linux distributions. On windows you will need to use [Cygwin](https://cygwin.com/install.html).

Let's say you want to forward UDP port 1054 on machine A.

On Machine A:
---

[Optional] If you want to start a UDP server to test:

.. code-block::
    socat up4-listen:1054,reuseaddr,fork -


Forward UDP traffic to a TCP port (6667 in this example):

.. code-block::
    socat tcp4-listen:127.0.0.1:6667,fork,reuseaddr udp4:localhost:1054

Start openport to forward the TCP port to the outside world:

.. code-block::
    openport 6667  --ip-link-protection=False

Example output:

.. code-block::
    INFO Now forwarding remote port openport.io:23607 to localhost:6667

Remember the port you got from openport (23607 in this example).

On Machine B:
---

Here we listen on UDP port 2054 (but it can be the same port as on machine A if you want)

.. code-block::
    socat udp4-listen:2054,reuseaddr,fork tcp:openport.io:23607

[Optional] If you want to test the forward:

.. code-block::
    echo "test" | socat - udp4:127.0.0.1:2054 ; echo $?
