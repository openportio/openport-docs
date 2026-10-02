Known limitations
=================

Http forwarding
---------------
WebSockets, streaming, long-lived connections and all HTTP methods work over
``--http-forward``, and you can serve the session on your own domain (see
:doc:`custom_domains`). A custom domain needs a real CNAME record, so apex
domains and proxied or flattened DNS records cannot be used, and a
custom-domain forward is reachable over https only.

UDP
---
Since client 2.3.0, a session forwards UDP next to TCP on the same port; the
status line shows ``(tcp & udp)`` when the server supports it. On older
clients, see :doc:`recipes_forwarding_udp` for a workaround with socat.
