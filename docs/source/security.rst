.. _security:

Security
========

Openport makes a port on your machine reachable from the internet. That is
the point of the product, and it is also a risk you should understand before
you rely on it. This page describes what protects a session, what Openport can
and cannot see, and what remains your own responsibility.

At a glance
-----------

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Question
     - Answer
   * - Who can reach my port?
     - By default only IP addresses that opened the session's
       :ref:`open-for-ip link <security_ip_link_protection>`. An
       :doc:`http forward <http_forwarding>` is different: anyone who knows
       its address can reach it.
   * - Is the tunnel encrypted?
     - Yes. The client connects to the Openport servers over SSH, or over TLS
       websockets with ``--ws``, and verifies the server's identity.
   * - Can Openport read my traffic?
     - Only as far as the protocol you tunnel is itself unencrypted. See
       `What Openport can see`_.
   * - Can my devices reach each other?
     - Not unless an organisation admin allows it. See
       `Connections between your devices`_.
   * - How do I know a download is genuine?
     - Packages and release checksums are signed. See `Verifying downloads`_.


Who can reach your port
-----------------------

**Open-for-ip link.** Every session is protected by an open-for-ip link unless
you turn that off. The port on the Openport server drops all traffic until
someone opens the link; from then on that visitor's IP address, and only that
one, can connect for 24 hours. See :doc:`security_ip_link_protection`.

**Connection rate limit.** You can cap the number of new TCP connections per
minute that a single IP address may make to a device's ports, which slows down
password guessing against whatever is listening behind the tunnel. It is off
by default. See :doc:`security_brute_force_blocking`.

**Http forwards.** A session started with ``--http-forward`` also gets a
hostname such as ``axkwjptb.u.openport.io``. That hostname is *not* gated by
the open-for-ip link and has no login of its own: anyone who knows the address
reaches your application. The address is hard to guess, and guessing is
rate-limited, but it is not a secret, because hostnames travel in clear text
in DNS lookups and TLS handshakes. Anything sensitive behind an http forward
needs its own authentication. The details are under "Access control" in
:doc:`http_forwarding`.

**Blocking a device.** A lost or compromised device can be blocked from the
`keys page <https://openport.io/user/keys>`_. Its sessions are cut immediately
and it cannot open new ones until you unblock it. See
:doc:`recipes_blocking_keys`.


The connection to the Openport servers
--------------------------------------

The client opens an outgoing connection to an Openport server and the server
relays traffic for your port over it. Nothing needs to listen for incoming
connections on your machine or your router.

- The default transport is **SSH**. Since client 2.3.0 the client verifies the
  server's SSH host key: the expected key is delivered over certificate-checked
  https, and a key that changes compared to an earlier connection is reported
  even then. Older clients accepted any host key; upgrade if you still run
  one.
- With ``--ws`` the transport is **websockets over TLS**, with the usual
  certificate validation. This is the option for networks that only allow
  outgoing https.
- ``--no-ssl`` turns that encryption off for the websocket transport. Anyone
  on the network path can then read and change everything in the tunnel. It
  exists for debugging on a network you fully trust and should never be used
  otherwise.


Device keys
-----------

Each machine identifies itself with its own key pair in ``~/.openport``. The
private key never leaves the machine; the public key is what you see on the
`keys page <https://openport.io/user/keys>`_.

- New keys are 4096-bit RSA (since client 2.3.0).
- The client generates a dedicated key. It does not reuse your personal
  ``~/.ssh`` key, as older versions did.
- A weak key left behind by an old client is replaced when you run
  ``openport register``, and ``openport rotate-key <your-token>`` replaces a
  key at any time while keeping your reserved ports.
- Anyone who can read ``~/.openport`` on a machine can open sessions as that
  device. Protect it like you protect ``~/.ssh``, and block the key if a
  machine is lost.

The registration token on the keys page links new devices to your account.
Treat it as a credential: do not commit it to a repository or bake it into an
image that others can download.


What Openport can see
---------------------

It helps to be precise about this, because the answer depends on how you use
the service.

- **Plain TCP and UDP ports** (``openport 22``): the server relays the bytes
  as they are. If the protocol is encrypted end to end, as ssh, https, RDP or
  a VPN are, the server only sees ciphertext. If it is not, as with plain
  http, telnet or an unencrypted database connection, the traffic is readable
  on the server and on the path between the visitor and the server. Prefer
  encrypted protocols.
- **Http forwards** (``--http-forward``): https from the visitor ends on the
  Openport server, which proxies the request to your tunnel. The server
  therefore handles the decrypted request and response.
- **TLS passthrough** (``--tls-passthrough``): TLS ends on your own machine.
  The server routes the connection by its hostname and relays encrypted bytes
  it cannot decrypt, and the certificate's private key never leaves your
  machine. Use this when the content must not be readable by Openport. See
  :doc:`custom_domains`.

Openport does record connection metadata needed to run the service, such as
which sessions exist, how much data they transfer, and which IP addresses
opened an open-for-ip link. The last of these is available to you through the
:ref:`API <api-open-for-ip-link-click>`.


Connections between your devices
--------------------------------

``openport connect``, ``openport ssh`` and ``openport forward`` let one device
on your account reach a port shared by another. Devices often sit in networks
you do not control, so this is **off by default**. An organisation admin
enables it with **Allow forward tunnels** on the
`organisation page <https://openport.io/user/organisation/>`_.

Once enabled, any device in the organisation can reach any other. To narrow
that down, put devices in device-groups: a device that is in a device-group
can only connect to devices in one of its own groups. See
:doc:`recipes_create_a_forward_tunnel`.

Device-groups also limit what people see: a user who is assigned to
device-groups only sees and manages the devices in those groups, while admins
see everything. See :ref:`key_groups`.


Verifying downloads
-------------------

- **apt repository.** The repository at ``https://openport.io/apt`` is signed,
  and apt verifies every package against the Openport archive key before
  installing it. The key's fingerprint is::

      1997 A054 2A55 8442 DE57  EAEA ED7C 9148 1AB0 0B20

- **Other downloads.** Each
  `release on GitHub <https://github.com/openportio/openport-go/releases>`_
  carries a ``SHA256SUMS.asc`` file: the checksums of every asset, signed with
  the same key. To check a download:

  .. code-block::

      $ curl -fsSL https://openport.io/apt/openport-archive-keyring.gpg | gpg --import
      $ gpg --decrypt SHA256SUMS.asc | sha256sum --ignore-missing -c -

  ``gpg`` must report a good signature from the fingerprint above, and
  ``sha256sum`` must print ``OK`` for the file you downloaded.

- **Software bill of materials.** Every release includes SBOMs (CycloneDX and
  SPDX) for the binaries, in the ``openport-apt-<version>.tar.gz`` asset.
- **Source.** The client is open source, so you can read what it does or
  :doc:`build it yourself <build_from_source>`.

Security fixes go into the latest release. Installing from the apt repository
means they arrive with your normal system updates (see :ref:`installation`).


What stays your responsibility
------------------------------

Openport controls who can reach the port. It does not secure the service that
listens behind it.

- Require authentication on anything you share, and use strong credentials.
  For ssh, prefer keys over passwords.
- Keep the shared service patched. A vulnerable web application is just as
  vulnerable through a tunnel.
- Share open-for-ip links only with the people who need them, and leave the
  protection on unless you have a reason to disable it.
- Remember what is running. Sessions started with ``--restart-on-reboot`` come
  back after a reboot; review them on the
  `sessions page <https://openport.io/user/sessions>`_ and stop what you no
  longer need (see :doc:`faq_how_can_i_stop_a_share`).
- Run the client as a normal user, not as root.


Reporting a vulnerability
-------------------------

Mail **security@openport.io**. Please do not open a public issue or post in
the support chat. We acknowledge reports within 5 business days and coordinate
disclosure with you; the full policy, including scope and response targets, is
in `SECURITY.md <https://github.com/openportio/openport-go/blob/main/SECURITY.md>`_.


See also
--------

.. toctree::

    security_ip_link_protection
    security_brute_force_blocking
