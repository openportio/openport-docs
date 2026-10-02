Release notes
=============

Install the client from the apt repository (see :ref:`installation`) or download
it from `Github <https://github.com/openportio/openport-go/releases>`_

Latest Version
--------------

2.3.0
+++++

Features/improvements:

- TLS passthrough with end-to-end encryption: ``--tls-passthrough`` terminates
  TLS on your machine instead of on the Openport servers, with automatic
  Let's Encrypt certificates or your own via ``--tls-cert`` / ``--tls-key``.
- Bring your own domain with ``--domain``: serve an http-forwarded session on a
  (sub)domain you own. See :doc:`custom_domains`.
- ``--local-tls`` and ``--local-proxy-protocol`` to let a local https server
  terminate TLS itself and still see the visitor's real IP.
- UDP forwarding: sessions now forward UDP next to TCP on the same port
  (also over the websocket transport). The status line shows ``(tcp & udp)``
  when the server supports it.
- Signed apt repository, so updates arrive through ``apt upgrade``
  (see :ref:`installation`).
- New ``rotate-key`` command to replace a machine's key; reserved ports are
  carried over.
- Stronger keys: new keys are 4096-bit, the client no longer adopts the user's
  personal SSH key, and weak keys are rotated on ``register``.
- The client now verifies the server's SSH host key.
- A loud warning when using ``--no-ssl``.

Bugfixes:

- ``--exit-on-failure-timeout`` now always fires when the connection fails.
- Patched reachable denial-of-service vulnerabilities in the ssh library
  (GO-2026-6354, GO-2026-6355).

Various:

- Replaced the cgo sqlite driver with a pure-Go one, improving portability.
- Releases are now built with goreleaser; rpm, mac and windows artifacts are
  published with a signed SHA256SUMS file.
- Security scanning and SBOM generation in CI.

Previous Versions
-----------------

2.2.3
+++++

- Fixed a panic when multiple routines write to the same websocket channel
  (`issue #6 <https://github.com/openportio/openport-go/issues/6>`_).
- Fixed restarting sessions that were created by client version 1.3.0.
- The ``selftest`` command no longer stores its sessions in the local database.
- Updated to golang 1.25 and updated all dependencies.

2.2.2
+++++

- Dependency upgrades (golang.org/x/crypto, golang.org/x/net, gorilla, ...).
- Stability fixes around the stored connection state.

2.2.1
+++++

- Added the ``selftest`` command.
- Fixed the error "user: lookup userid 0: invalid argument".

2.2.0
+++++

Features/improvements:

- Added --ws flag to use websockets to connect instead of ssh. (Use in combination with --no-ssl to create an unencrypted tunnel).
- Improved help messages.
- adding the hostname to the ssh keys when creating new keys.
- added "rm" command to remove sessions from the local database.
- "register", "restartsessions", "link" are now also valid commands
- Added an improved windows service.
- Smaller binaries by removing debug flags from compilation.
- Using static linking to improve portability.

Bugfixes:

- Fixed "(no such table: sessions)" error
- Flagging sessions as automatically restarted when they are started from "restart-sessions"
- Fixed database issues.

Various:

- Upgraded to golang version 1.21.5
- Also releasing the raw binaries
- The armv7 binary have been tested on an openwrt router.


2.1.0
+++++++++
- Adding --exit-on-failure-timeout flag (Specify in seconds if you want the app to exit if it cannot properly connect. (default -1))
- Code refactoring


2.0.4
+++++++++
- Fixed issue with restarting sessions from a non-root user.
- Creating the /etc/openport/users.conf file at installation.


2.0.3
+++++++++

- Checking that both .ssh/id_rsa and .ssh/id_rsa.pub exist before using them
- Bugfix: server port was not reused after closing the app with ctrl-c


2.0.2
+++++++++

- Rewritten client in Go
- Updated commands: "openport --list" is now "openport list"
  Same for forward, list, kill, kill-all, register-key, version and help.
- Fixed issue with hanging clients (added timeout on http requests)
- Improved speed, size and memory consumption
- Terminology: replaced "share" with "session"


1.2.0
+++++++++

- Fixed ssl issue in ubuntu
- Python3 compatibility
- Dependency upgrades
- Added --keep-alive flag to modify the interval in between the keep-alive messages

1.1.0
+++++++++

- Added --daemonize flag
- Fixed issue with saving session without an internet connection
- Added open-for-ip link in --list output


1.0.2
+++++++++

- Moved openport to it's own package, so it can be installed as a regular python package.


1.0.1
+++++++++

- added --name to --register-key

1.0.0
+++++++++

- Added a Graphical User Interface (GUI)
- ip-link-protection can be switched on and off from the command line
- You can now create [forward tunnels](/wiki/recipes/create_a_forward_tunnel/)
- Many bugfixes
- Removed the manager
- Only one forward per port
- Can handle encrypted keys
- 64 bit version for ubuntu
- Better warnings and messages
- Better signal handling
- Show extra information with --list --verbose

0.9.1
+++++++++

- IP Link Protection
- Brute force protection
- Various bug fixes
- Cleaner output


0.8.0
+++++++++

- Port forwarding
- Http forwarding
- Restart on reboot
