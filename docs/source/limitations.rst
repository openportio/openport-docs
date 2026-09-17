Known limitations
=================

Http forwarding
---------------
The ``--http-forward`` hostname (see :doc:`http_forwarding`) is always randomly
generated; custom or vanity domains are not supported. WebSockets, streaming,
long-lived connections and all HTTP methods do work over it.

UDP
---
Openport forwards TCP traffic. UDP-based services are not supported out of the
box, but see :doc:`recipes_forwarding_udp` for a workaround with socat.
