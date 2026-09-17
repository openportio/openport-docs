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

Use the resulting ``https://<xxxxx>.u.openport.io`` address as the external
URL, and add that hostname to Home Assistant's ``http:`` configuration
(``use_x_forwarded_for`` / ``trusted_proxies``, and ``cors_allowed_origins``
if needed). Home Assistant provides its own login, so this pairs well with
disabling the ip-link protection for that key (see `Access control`_).

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
- ``X-Forwarded-Proto``: ``http`` or ``https``.
- ``Host``: the public forwarded hostname (``<xxxxx>.u.openport.io``). If your
  application maintains a list of allowed hosts, add this hostname to it.

Access control
--------------

The :ref:`open-for-ip-link <open-for-ip-link>` protection applies to
http-forwarded sessions like it does to regular sessions. While it is active,
visiting the forwarded address before clicking the link returns **403
Forbidden**; after the visitor's IP has clicked the link the site is reachable
for 24 hours. It can be disabled with ``--ip-link-protection False`` (or per
key on the Keys page) — do that when the address changes between visitors or
networks (for example a phone on mobile data) and your application has its own
login.

There is no login page or client-certificate check on the forwarded hostname
itself, so an application without ip-link protection must provide its own
authentication.

Limitations
-----------

- The hostname is always randomly generated. Custom or vanity domains are not
  supported.

Everything else a web application typically needs works: HTTPS with a valid
certificate, WebSockets, streaming/Server-Sent Events, chunked responses,
long-lived connections and all HTTP methods.
