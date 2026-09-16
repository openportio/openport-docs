Known limitations
=================

Http forwarding
---------------
The ``--http-forward`` proxy (see :doc:`http_forwarding`) is a simple
request/response proxy:

- **WebSockets are not supported.** The connection upgrade does not pass
  through the proxy, so applications that rely on a WebSocket (chat apps,
  Home Assistant, live dashboards, ...) will not work over the forwarded
  hostname.
- Server-Sent Events, streaming and chunked responses are not supported.
  Responses are buffered and delivered in one piece.
- Request bodies are only forwarded for ``POST`` requests. ``PUT``, ``PATCH``
  and ``DELETE`` requests are forwarded, but their bodies are dropped.
- Requests that take longer than 30 seconds are cut off.
- The hostname is always randomly generated; custom domains are not supported.

None of these limitations apply to the regular TCP port of the session (the
``openport.io:<port>`` address): that path forwards raw bytes, so WebSockets,
streaming and all HTTP methods work there. The trade-offs of the raw port are
the changing port number and the lack of a matching TLS certificate.

UDP
---
Openport forwards TCP traffic. UDP-based services are not supported out of the
box, but see :doc:`recipes_forwarding_udp` for a workaround with socat.
