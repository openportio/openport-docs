Release notes
=============

Download the client from `Github <https://github.com/openportio/openport-go/releases>`_

2.2.0: Latest
--------------
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
--------------
- Adding --exit-on-failure-timeout flag (Specify in seconds if you want the app to exit if it cannot properly connect. (default -1))
- Code refactoring


2.0.4
-------------------------
- Fixed issue with restarting sessions from a non-root user.
- Creating the /etc/openport/users.conf file at installation.


2.0.3
-----
- Checking that both .ssh/id_rsa and .ssh/id_rsa.pub exist before using them
- Bugfix: server port was not reused after closing the app with ctrl-c


2.0.2
-----
- Rewritten client in Go
- Updated commands: "openport --list" is now "openport list"
  Same for forward, list, kill, kill-all, register-key, version and help.
- Fixed issue with hanging clients (added timeout on http requests)
- Improved speed, size and memory consumption
- Terminology: replaced "share" with "session"


1.2.0
-----
- Fixed ssl issue in ubuntu
- Python3 compatibility
- Dependency upgrades
- Added --keep-alive flag to modify the interval in between the keep-alive messages

1.1.0
-----
- Added --daemonize flag
- Fixed issue with saving session without an internet connection
- Added open-for-ip link in --list output


1.0.2
-----
- Moved openport to it's own package, so it can be installed as a regular python package.


1.0.1
-----
- added --name to --register-key

1.0.0
-----
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
-----
- IP Link Protection
- Brute force protection
- Various bug fixes
- Cleaner output


0.8.0
-------
- Port forwarding
- Http forwarding
- Restart on reboot
