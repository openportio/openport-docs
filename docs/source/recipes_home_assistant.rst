Remote access to Home Assistant
===============================

The `Openport Home Assistant add-on <https://github.com/jandebleser/home-assistant-addons>`_
exposes your Home Assistant UI on a stable ``https://<name>.u.openport.io``
address using an http-forward tunnel — no port forwarding, no static IP, no
VPN setup. WebSockets are fully supported, so the Home Assistant frontend and
the companion mobile apps work out of the box.

Installation
------------

#. In Home Assistant, go to **Settings → Add-ons → Add-on Store**.
#. Open the ⋮ menu (top right) → **Repositories**.
#. Add ``https://github.com/jandebleser/home-assistant-addons`` and press **Add**.
#. Install the **Openport** add-on from the store.

Setup
-----

#. Create an account at `openport.io <https://openport.io>`_ if you don't have one.
#. Go to https://openport.io/user/keys and copy your **key registration token**.
#. Paste the token into the add-on's ``key_registration_token`` option.
#. Allow proxied requests in Home Assistant: the tunnel reaches your
   installation through a local proxy, so ``configuration.yaml`` needs:

   .. code-block:: yaml

       http:
         use_x_forwarded_for: true
         trusted_proxies:
           - 127.0.0.1

   Restart Home Assistant after adding this. Without it, requests through the
   tunnel fail with ``400: Bad Request``.
#. Start the add-on and open the log. After a few seconds it prints your
   public address, e.g.::

       Now forwarding remote address abcde.u.openport.io to localhost

   Your Home Assistant is now reachable at ``https://abcde.u.openport.io``.
#. Tell Home Assistant about its new external address: go to
   **Settings → System → Network** and set the **External URL** to
   ``https://<xxxxx>.u.openport.io``. The companion apps can use this URL as
   their server address.

The address is tied to the add-on's stored session and key, so it stays the
same across restarts, reboots, and add-on updates.

Since your Home Assistant login page becomes reachable from the internet, make
sure every user has a strong password, and consider enabling
`multi-factor authentication <https://www.home-assistant.io/docs/authentication/multi-factor-auth/>`_.

See the `add-on documentation <https://github.com/jandebleser/home-assistant-addons/blob/master/openport/DOCS.md>`_
for all options and troubleshooting.
