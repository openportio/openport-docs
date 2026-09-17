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
    You are now connected. Your local port 8080 is now available on http://axkwjptb.u.openport.io

The hostname has the form ``<8 random letters>.u.<server>``, for example
``axkwjptb.u.openport.io`` or ``axkwjptb.u.spr.openport.io``, depending on which
server your session lands on. The random part is a secret, so the address
cannot be guessed.

Requests to this hostname are accepted on the standard ports:

- **http** (port 80)
- **https** (port 443), with a valid wildcard certificate — no browser
  warnings, no certificate setup on your side.

The Openport server looks at the hostname of each incoming request and proxies
it to the tunnel of the matching session, which delivers it to your local port.
The connection is passed through transparently, so **WebSockets, streaming
responses (Server-Sent Events, chunked transfers) and every HTTP method work**,
and long-lived connections stay open. This makes it suitable for full web
applications, not just simple request/response sites.

Home Assistant
--------------

Because WebSockets are supported, you can reach a `Home Assistant
<https://www.home-assistant.io/>`_ instance (including its
``/api/websocket`` connection) from anywhere:

.. code-block::

    openport 8123 --http-forward -R

Use the resulting ``https://<xxxxxxxx>.u.openport.io`` address as the external
URL, and add that hostname to Home Assistant's ``http:`` configuration
(``use_x_forwarded_for`` / ``trusted_proxies``, and ``cors_allowed_origins``
if needed). No open-for-ip-link click is needed to reach it (see `Access
control`_); Home Assistant provides its own login on top.

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
  (``<xxxxxxxx>.u.openport.io``).
- ``X-Forwarded-Proto``: ``http`` or ``https``.
- ``Host``: the public forwarded hostname (``<xxxxxxxx>.u.openport.io``). If your
  application maintains a list of allowed hosts, add this hostname to it.

Access control
--------------

The forwarding address is itself the secret: anyone who knows the full
``<8 random letters>.u.openport.io`` address can reach your application, and
**no open-for-ip-link click is required** (the :ref:`open-for-ip-link
<open-for-ip-link>` protection only gates the raw ``openport.io:<port>``
address, not the http-forward hostname). The address cannot be guessed — it is
8 random letters, and the server rate-limits requests to unknown addresses to
one per second per IP.

There is no login page or client-certificate check on the forwarded hostname,
so treat the address as a secret and give the application its own
authentication if the data behind it is sensitive.

Limitations
-----------

- The hostname is always randomly generated. Custom or vanity domains are not
  supported.

Everything else a web application typically needs works: HTTPS with a valid
certificate, WebSockets, streaming/Server-Sent Events, chunked responses,
long-lived connections and all HTTP methods.
