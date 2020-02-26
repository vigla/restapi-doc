.. http:post:: /address/new/

    Generates a new subaddress and starts monitoring it immediately

    **Example request**:

    .. sourcecode:: python

        requests.post(
            "https://api.vigla.biz/address/new/",
            auth=HTTPBasicAuth(
                "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
                "b690d2d2-80cf-41f1-a2c8-29972c076d24"))

    **Example response**:

    .. sourcecode:: json

        {
          "account_index": 0,
          "active": true,
          "active_until": null,
          "address": "79qwF2b6r5VA9oCJwwX5TdPeGtUqkWfBTfivzQ5yRrWDiGNAnCPWc98dhufqonDjMce65HMrajDV94QPk4frvCGaKJqEVpD",
          "subaddr_index": 257
        }

    :<json int account_index: the index of wallet's account for which the new address has to be
        generated; default is ``0``, the main account
    :<json int expires_in: the address expiration time in seconds; default is ``null`` which means
        no expiration
    :resheader Content-Type: application/json
    :statuscode 200: no error
    :statuscode 401: address and access token don't match
    :>json int account_index: the index of wallet's account the address belongs to (also known as *major index*)
    :>json boolean active: flag indicating whether the address is being actively monitored for
        incoming payments
    :>json datetime active_until: the time when address becomes inactive (in UTC) or ``null`` when
        it has no expiration time
    :>json string address: the newly generated address itself
    :>json int subaddr_index: the index of the subaddress within the wallet's account (also known as *minor index*)
