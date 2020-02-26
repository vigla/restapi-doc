.. http:get:: /address/(string:address)/

    Returns the status of existing subaddress

    **Example request**:

    .. sourcecode:: python

        requests.get(
            "https://api.vigla.biz/address/75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa/",
            auth=HTTPBasicAuth(
                "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
                "b690d2d2-80cf-41f1-a2c8-29972c076d24"))

    **Example response**:

    .. sourcecode:: json

        {
          "account_index": 0,
          "active": true,
          "active_until": null,
          "address": "75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa",
          "subaddr_index": 265
        }

    :resheader Content-Type: application/json
    :statuscode 200: no error
    :statuscode 401: address and access token don't match
    :statuscode 404: the wallet doesn't contain specified subaddress
    :>json int account_index: the index of wallet's account the address belongs to (also known as *major index*)
    :>json boolean active: flag indicating whether the address is being actively monitored for
        incoming payments
    :>json datetime active_until: the time when address becomes inactive (in UTC) or ``null`` when
        it has no expiration time
    :>json string address: the subaddress itself
    :>json int subaddr_index: the index of the subaddress within the wallet's account (also known as *minor index*)


.. http:put:: /address/(string:address)/

    Modifies existing subaddress. Currently it's only possible to modify the expiration time or to
    disable the expiration completely.

    **Example request**:

    .. sourcecode:: python

        requests.put(
            "https://api.vigla.biz/address/75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa",
            data={"expires_in": 60*60*24,
            auth=HTTPBasicAuth(
                "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
                "b690d2d2-80cf-41f1-a2c8-29972c076d24"))

    **Example response**:

    .. sourcecode:: json

        {
          "account_index": 0,
          "active": true,
          "active_until": "2020-02-22T14:29:22.277227",
          "address": "75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa",
          "subaddr_index": 265
        }

    :<json int expires_in: the address expiration time in seconds; negative value disables
        expiration, while ``0`` makes the address expire immediately
    :resheader Content-Type: application/json
    :statuscode 200: no error
    :statuscode 401: address and access token don't match
    :statuscode 404: the wallet doesn't contain specified subaddress
    :>json int account_index: the index of wallet's account the address belongs to (also known as *major index*)
    :>json boolean active: flag indicating whether the address is being actively monitored for
        incoming payments
    :>json datetime active_until: the time when address becomes inactive (in UTC) or ``null`` when
        it has no expiration time
    :>json string address: the subaddress itself
    :>json int subaddr_index: the index of the subaddress within the wallet's account (also known as *minor index*)
