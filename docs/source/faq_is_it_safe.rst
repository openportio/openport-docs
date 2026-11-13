Is it safe
==========

Openport is built so that opening a port does not mean opening it to everyone:

- A port accepts no traffic until a visitor has opened its
  :ref:`open-for-ip link <security_ip_link_protection>`, and then only from
  that visitor's IP address. You can change this per session or per device.
- The connection between the client and the Openport servers is encrypted,
  and the client verifies which server it is talking to.
- You can :ref:`limit the number of connections per minute <security_brute_force_blocking>`
  to slow down password guessing.
- With :doc:`TLS passthrough <custom_domains>` the traffic stays encrypted all
  the way to your machine, so Openport cannot read it.
- Devices on your account cannot reach each other unless an admin allows it.
- Packages are signed, and the client is open source.

What Openport does not do is secure the service you share. A weak password or
an unpatched application is as exposed through a tunnel as it would be on a
public IP address, to everyone who is allowed to reach the port. Note also
that an :doc:`http forward <http_forwarding>` is reachable by anyone who knows
its address.

The :ref:`security page <security>` explains all of this in detail, including
what Openport can and cannot see.
