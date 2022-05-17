Using the API
==============

When you have a paying account, you will get access to the openport API.
Using this API, you can request a list of your clients, and active sessions.
Find the full specs `here <https://openport.io/api/v2/specs/>`.

Example response:


.. code-block::
    GET https://openport.io/api/v2/sessions/?token=SECRET
    {
        "count": 2,
        "next": null,
        "previous": null,
        "results": [
            {
                "key": "https://openport.io/api/v2/keys/15371/",
                "port": 6260,
                "local_port": 22,
                "server": "openport.io",
                "http_forwarding_address": null,
                "open_port_for_ip_link": "https://openport.io/l/6260/",
                "redirect_url": "https://openport.io/r/pdDrqgF7/22"
            },
            {
                "key": "https://openport.io/api/v2/keys/4642/",
                "port": 40497,
                "local_port": 8081,
                "server": "openport.io",
                "http_forwarding_address": null,
                "open_port_for_ip_link": "https://www.openport.io/l/40497/",
                "redirect_url": "https://www.openport.io/r/LDnIs6MF/8081"
            }
        }