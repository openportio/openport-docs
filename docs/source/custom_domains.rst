.. _custom-domains:

Bring your own domain
=====================

An http-forwarded session (see :doc:`http_forwarding`) normally lives on a
generated hostname like ``axkwjptb.u.openport.io``. Since client 2.3.0 you can
serve it on a (sub)domain you own instead, with a certificate for that domain:

.. code-block::

    $ openport 8080 --tls-passthrough --domain status.example.com

Two things change compared to a plain http forward:

- Visitors reach your service on ``https://status.example.com`` instead of the
  generated address.
- The TLS connection ends **on your machine**, not on the Openport servers.
  The server routes the connection by its SNI name and relays the encrypted
  bytes; it cannot decrypt the traffic passing through. The certificate and
  its private key never leave your machine.

Setting it up
-------------

1. Start the client:

   .. code-block::

       $ openport 8080 --tls-passthrough --domain status.example.com

   You don't need to prepare anything first. Until the DNS record below
   exists, the session behaves like a normal http forward on its generated
   address, and the client prints the exact record to create:

   .. code-block::

       To serve https://status.example.com with end-to-end encryption, create a CNAME
       record at your DNS provider. In most provider dashboards:
           Type:          CNAME
           Name/Host:     the subdomain part of status.example.com
                          (some providers want the full name; no trailing dot)
           Value/Target:  axkwjptb.u.openport.io
           TTL:           any (300 is fine)

2. Create that CNAME record at your DNS provider.

3. Wait a moment. The client checks DNS every 20 seconds; once the record
   resolves, it reconnects in passthrough mode, requests a certificate for
   your domain from Let's Encrypt, and starts serving
   ``https://status.example.com``. No restart is needed.

The certificate arrives through the tunnel itself (ACME ``tls-alpn-01``), so
no ports need to be open on your machine and no DNS API credentials are
needed. It is stored under ``~/.openport/certs`` and renewed automatically.

DNS requirements
----------------

The verification requires a real CNAME record, because the record pointing at
your session's forwarding address is what proves you control the domain:

- **Use a subdomain** (``status.example.com``), not the apex
  (``example.com``). Apex domains with CNAME flattening answer with A records
  and cannot be verified.
- **The record must be DNS-only.** Proxied records (for example Cloudflare's
  orange cloud) also answer with A records and cannot be verified.

The domain stays claimed by your account while the session is active, so
another account cannot take it over.

Bringing your own certificate
-----------------------------

If you already have a certificate for the domain, pass it instead of using
Let's Encrypt:

.. code-block::

    $ openport 8080 --tls-passthrough --domain status.example.com \
        --tls-cert fullchain.pem --tls-key privkey.pem

If the local service terminates TLS itself (an nginx or Caddy that already
serves https), use ``--local-tls`` to relay the encrypted bytes straight to
it:

.. code-block::

    $ openport 443 --tls-passthrough --local-tls --domain status.example.com

You then manage the certificate in that service. Renew it via DNS-01;
http-01 does not reach a passthrough forward. Add ``--local-proxy-protocol``
if the service understands the PROXY protocol (for example nginx with
``listen 443 ssl proxy_protocol``) and you want it to see the visitor's real
IP address.

End-to-end encryption without a domain
--------------------------------------

``--tls-passthrough`` also works without ``--domain``: with ``--tls-cert`` /
``--tls-key`` or ``--local-tls``, the session keeps its generated
``<xxxxxxxx>.u.openport.io`` address but TLS still terminates on your machine
instead of on the Openport servers. Browsers will warn unless the certificate
you provide covers that address; this mode is mostly useful for API clients
that pin your certificate.

Notes
-----

- A passthrough forward is **https only**: the server cannot inject plain
  port-80 traffic into a tunnel that carries TLS, so visitors must use
  ``https://``. Port 80 on the domain is not forwarded.
- Because the server cannot see inside the connection, the headers from
  :doc:`http_forwarding` (``X-Forwarded-For`` and friends) are **not** added:
  your service sees plain requests coming from localhost. If you need the
  visitor's IP address, terminate TLS in your own service with
  ``--local-tls --local-proxy-protocol``.
- The requirements of http forwards apply: the key must be registered to an
  account, and the :ref:`open-for-ip-link <open-for-ip-link>` notes from
  :doc:`http_forwarding` apply unchanged.
