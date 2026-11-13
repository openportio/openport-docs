.. _security_brute_force_blocking:

Connection rate limit
=====================

A port that is reachable from the internet invites password guessing. To slow
that down, you can limit how many new TCP connections a single IP address may make
to a device's ports per minute. Connections above the limit are dropped by the
Openport server before they reach your machine.

The limit is **off by default**. To set it:

#. Go to the `keys page <https://openport.io/user/keys>`_ and edit the device.
#. Enter a value for **Max connections per minute**, for example ``10``.
#. Save. The limit applies to the sessions of that device.

Entering ``0`` turns the limit off again.

Notes:

- The limit counts new connections per visiting IP address, so one noisy
  visitor does not lock out the others.
- It applies to the session's port (``openport.io:<port>``). Requests to an
  :doc:`http-forward <http_forwarding>` hostname do not pass through it.
- Pick a value that fits the service. Ssh and remote desktop need very few new
  connections per minute; a web application reached on its raw port opens
  many, and a low limit will break it.
- A rate limit slows an attacker down, it does not replace authentication.
  Combine it with the :ref:`open-for-ip link <security_ip_link_protection>`
  and strong credentials on the service itself.
