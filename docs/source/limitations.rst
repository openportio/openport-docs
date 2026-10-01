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
Openport forwards TCP traffic. UDP-based services are not supported out of the
box, but see :doc:`recipes_forwarding_udp` for a workaround with socat.
