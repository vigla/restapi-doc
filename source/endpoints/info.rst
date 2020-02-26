.. http:get:: /info/

    Returns the basic wallet info

    **Example request**:

    .. sourcecode:: python

        requests.get(
            "https://api.vigla.biz/info/",
            auth=HTTPBasicAuth(
                "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
                "b690d2d2-80cf-41f1-a2c8-29972c076d24"))

    **Example response**:

    .. sourcecode:: json

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

    :resheader Content-Type: application/json
    :statuscode 200: no error
    :statuscode 401: address and access token don't match
    :>json datetime active_until: time when wallet becomes inactive (in UTC)
    :>json object addresses: address stats
    :>json int addresses.all: number of all wallet's addresses
    :>json int addresses.active: number of addresses being monitored for new payments
    :>json datetime created: time when the wallet was created (in UTC)
    :>json string master_address: the main address of the wallet
    :>json string name: wallet name
    :>json string net: the net wallet works in (one of ``main``, ``test`` or ``stage``)
    :>json string notify_url: the address where notifications about payments are sent
    :>json boolean watch_master: whether the master address is being monitored for payments
