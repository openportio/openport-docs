.. _http-forwarding:

HTTP forwarding
===============

By default, Openport gives your session a raw TCP port on one of the Openport
servers (for example ``openport.io:38261``). That works for ssh, VNC or any
other TCP service, but it is inconvenient for web applications: the port number
can change between sessions, and there is no TLS certificate for
``openport.io:38261``, so browsers will show warnings if your local service
speaks https.

The ``--http-forward`` option solves this for web applications. Next to the
regular TCP port, your session gets its own hostname:

.. code-block::

    $ openport 8080 --http-forward
    ...
    You are now connected. Your local port 8080 is now available on http://axkwj.u.openport.io

The hostname has the form ``<5 random letters>.u.<server>``, for example
``axkwj.u.openport.io`` or ``axkwj.u.spr.openport.io``, depending on which
server your session lands on.

Requests to this hostname are accepted on the standard ports:

- **http** (port 80)
- **https** (port 443), with a valid wildcard certificate — no browser
  warnings, no certificate setup on your side.

The Openport server looks at the hostname of each incoming request and proxies
it to the tunnel of the matching session, which delivers it to your local port.

Requirements
------------

- Your key must be registered to an account (see
  :ref:`Linking a key to your account <register-key>`). Unregistered keys
  cannot request http forwards. A free account is sufficient.

Is the address stable?
----------------------

Yes, in the same way your port is (see the "Volatile ports" section in
:doc:`usage`): when the same client restarts a session for the same local
port, it keeps its forwarding address. You will get a *new* address if the
session cannot be matched to the previous one, for example when the key
changed or the local session database (``~/.openport``) was removed.

If you want a link that never changes, use the ``redirect_url`` from the
:ref:`sessions API <api-sessions>` (for example
``https://openport.io/r/pdDrqgF7/8080``). That link is fixed per key and
redirects to the current forwarding address of the session for that local
port.

Headers seen by your application
--------------------------------

The proxy sets the following headers on forwarded requests:

- ``X-Forwarded-For``: the IP address of the visitor.
- ``X-Forwarded-Host``: the public hostname the visitor used
  (``<xxxxx>.u.openport.io``).
- ``Host`` is rewritten to ``127.0.0.1:<your local port>``. If your
  application validates the ``Host`` header, allow this value or configure it
  to trust the ``X-Forwarded-Host`` header instead.

Access control
--------------

The :ref:`open-for-ip-link <open-for-ip-link>` protection applies to
http-forwarded sessions like it does to regular sessions, and can be disabled
the same way (``--ip-link-protection False`` or per key on the Keys page).
There is no login page or client-certificate check on the forwarded hostname:
anyone who can reach the address gets through to your application, so make
sure the application has its own authentication.

Limitations
-----------

HTTP forwarding is a simple request/response proxy. See
:doc:`limitations` for the full list; the highlights:

- **WebSockets are not supported.** Applications that need a WebSocket
  connection (for example Home Assistant's ``/api/websocket``) will not work
  through ``--http-forward``. Use the regular TCP port for those.
- Server-Sent Events, streaming and chunked responses are not supported;
  responses are buffered and delivered in one piece.
- Request bodies are only forwarded for ``POST`` requests. ``PUT``, ``PATCH``
  and ``DELETE`` requests are forwarded, but their bodies are dropped.
- Long-running requests are cut off after 30 seconds.
- The hostname is always randomly generated. Custom or vanity domains are not
  supported.

If you hit any of these, you can always fall back to the raw TCP port of the
same session — that path proxies bytes without interpreting them, so
WebSockets, streaming and all HTTP methods work there.
