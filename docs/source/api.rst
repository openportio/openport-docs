API
===

To access the API, you need to have a paying account.
Using this API, you can request a list of your clients, active sessions, etc.
Find the full OpenAPI specs `here <https://api.openport.io/api/v2/specs/>`_.


Sessions
--------

Supported methods:

- GET

.. code-block::

    GET https://api.openport.io/api/v2/sessions/?token=SECRET
    {
        "count": 2,
        "next": null,
        "previous": null,
        "results": [
            {
                "key": "https://api.openport.io/api/v2/keys/15371/",
                "port": 6260,
                "local_port": 22,
                "server": "openport.io",
                "http_forwarding_address": null,
                "open_port_for_ip_link": "https://api.openport.io/l/6260/",
                "redirect_url": "https://api.openport.io/r/pdDrqgF7/22"
            },
            {
                "key": "https://api.openport.io/api/v2/keys/4642/",
                "port": 40497,
                "local_port": 8081,
                "server": "openport.io",
                "http_forwarding_address": null,
                "open_port_for_ip_link": "https://www.openport.io/l/40497/",
                "redirect_url": "https://www.openport.io/r/LDnIs6MF/8081"
            }
        }

Keys
----
Supported Methods:

- GET
- POST
- PUT
- PATCH
- DELETE


.. code-block::

    GET https://api.openport.io/api/v2/keys/?token=SECRET
    {
        "count": 1044,
        "next": "https://api.openport.io/api/v2/keys/?limit=100&offset=100&token=SECRET",
        "previous": null,
        "results": [
            {
                "url": "https://api.openport.io/api/v2/keys/349/",
                "public_key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAAABAAAAgQCkcRNDS46uH5Q4cUbEmG+A9mLWXq9IpLiXxzGNYrQ+DYsyzh8qf3ZmG/L+/R3Gu0eY82AtfPLlDAvI9+DrbWAAmK7Qap5l+2vyiZ2zCNgKS0jVOw4R2yGt1kOQqd/mypSSAbxbcYrbPvtAoDSlR7RmVikQ+hfFjgvtCTVrBI7ijQ== root",
                "name": "root@HQ",
                "id": 349,
                "last_connection_time": "2022-06-19T16:29:25+02:00",
                "creation_time": "2016-11-13T00:18:19+01:00",
                "redirect_token": "tNGHAEe",
                "bytes_this_month": 0,
                "counter_reset_time": "2023-04-11T06:17:26.261628+02:00"
            },
            {
                "url": "https://api.openport.io/api/v2/keys/377/",
                "public_key": "ssh-rsa AAAAB3NzaC1yc2EAAAADAAQBAAAAgQC5mBOBARdDEkNThKZ623EEy52rgEKzhqJdtRjz5OPba1tzgOf0VbrsYRykaZyyJ+HnvByJzFgjdClFaSxBO8c8L0gNlsDd+UOvR4NBP++TSl0bcbd+iXFgpJMM3H1TUyAUKEG6CJ6raPZriwzyG3y8acPRawtn+oWPT5oWsEw6bQ== pi",
                "name": "Pi London",
                "id": 377,
                "last_connection_time": "2023-03-28T21:17:08+01:00",
                "creation_time": "2014-06-04T18:05:23+02:00",
                "redirect_token": "NDXCcAa",
                "bytes_this_month": 0,
                "counter_reset_time": "2023-04-11T06:17:26.261628+02:00"
            }
    }


Open-For-IP Link Clicks
-----------------------

Supported methods:

- GET

Example response:

.. code-block::

    GET https://api.openport.io/api/v2/openforiplinkclicks/?token=SECRET
    {
    "count": 178268,
    "next": "https://api.openport.io/api/v2/openforiplinkclicks/?limit=100&offset=100&token=SECRET",
    "previous": null,
    "results": [
        {
            "port": 12345,
            "source_ip": "1.2.3.4",
            "timestamp": "2021-11-15T18:23:34.113431+01:00",
            "key_name": "EURO44"
        },
        {
            "port": 23456,
            "source_ip": "2.3.4.5",
            "timestamp": "2021-11-15T18:27:36.648734+01:00",
            "key_name": "USA45"
        },
        ...
    }



Server Nodes
------------

Supported methods:

- GET


.. code-block::

    GET https://api.openport.io/api/v2/nodes/
    {
        "count": 3,
        "next": null,
        "previous": null,
        "results": [
            {
                "name": "us",
                "public_ip": "104.131.142.252",
                "status": "active",
                "public_dns_name": "us.openport.io"
            },
            {
                "name": "singapore",
                "public_ip": "128.199.116.155",
                "status": "active",
                "public_dns_name": "spr.openport.io"
            },
            {
                "name": "openport-main-2",
                "public_ip": "95.85.25.182",
                "status": "active",
                "public_dns_name": "openport.io"
            }
        ]
    }

