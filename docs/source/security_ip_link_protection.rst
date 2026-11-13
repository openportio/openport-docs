.. _security_ip_link_protection:

Open-for-ip link protection
===========================

A freshly opened port accepts nobody. Before a visitor can connect, they have
to open a link that contains a secret token; that tells the Openport server to
let their IP address through. Port scanners and anyone else who merely finds
the port see it as closed.

When you start a session, the client prints the link:

.. code-block::

    $ openport 22
    Now forwarding remote port openport.io:12345 to localhost:22 .
    Tell the one you are sending this link, to first visit https://openport.io/l/12345/abscsqdfklmagsdgq . This will allow their IP to contact you.

Send that link to the person who needs access. Once they have opened it, they
can connect to ``openport.io:12345`` from the IP address they opened it from.
You can find the link again on the
`sessions page <https://openport.io/user/sessions>`_.

How it behaves
--------------

- Access is granted **per IP address** and lasts **24 hours** after the link
  was opened. After that the visitor opens the link again.
- Everyone behind the same public IP address, such as an office network, gets
  access together.
- The link is a credential. Whoever has it can let themselves in, so share it
  over a channel you trust.
- You can see who opened your links through the
  :ref:`API <api-open-for-ip-link-click>`.
- The protection covers the session's port. The hostname of an
  :doc:`http forward <http_forwarding>` is not gated by it.

Turning it off
--------------

For a service that must be reachable by anyone, for example a public web
server or a webhook endpoint, you can disable the protection:

- for one session, by starting it with ``--ip-link-protection False``;
- for all sessions of a device, by unticking **ip link protection** when
  editing the device on the `keys page <https://openport.io/user/keys>`_.

The port is then open to the whole internet, so make sure the service behind
it authenticates its users. Consider setting a
:ref:`connection rate limit <security_brute_force_blocking>` as well.
