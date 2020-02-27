Using the API
=============

In order to start using the REST API you need to:

 1. `Sign up`_ at Vigla,
 2. add a wallet,
 3. retrieve the ``access token`` form the wallet page.

The access token is a random UUID automatically generated when you add a wallet to Vigla.
**It should be kept secret** and for that reason it's hidden on the page by default.

.. warning:: Should you have any suspicions about your access token being disclosed to unauthorized person,
            please generate a new one and update your config.

.. _`Sign up`: https://vigla.biz/register/


Basic settings
--------------

The base URL for all API requests is ``https://api.vigla.biz/``

All input data is required to be in JSON format, and all responses are returned in JSON format.


Authentication
..............

The API requires `HTTP Basic auth`_ with:

 * *username*: the wallet's master address,
 * *password*: the ``access token``

.. _`HTTP Basic auth`: https://en.wikipedia.org/wiki/Basic_access_authentication


Making requests
---------------

Let's assume you have a wallet with:

 * address ``56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj``
 * access token ``b690d2d2-80cf-41f1-a2c8-29972c076d24``


An example request using `Python requests`_ library would look like:

.. code-block:: python

    import json
    import requests
    import requests.auth

    auth = requests.auth.HTTPBasicAuth(
        "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
        "b690d2d2-80cf-41f1-a2c8-29972c076d24")
    rsp = requests.post("https://api.vigla.biz/info/", auth=auth)

    print(json.dumps(rsp.json(), indent=2))

And example output would be:

.. code-block:: json

    {
      "active_until": "2020-05-18T11:00:26.743958",
      "addresses": {
        "active": 65,
        "all": 76
      },
      "created": "2020-02-18T11:00:26.744348",
      "height": 0,
      "master_address": "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
      "name": "test order payments",
      "net": "stage",
      "notify_url": "http://my.shop.url/vigla-notify/",
      "watch_master": false
    }

With the connection properly set up, it's time to :doc:`obtain your first address <subaddresses>`.

.. _`Python requests`: https://2.python-requests.org/en/master/
