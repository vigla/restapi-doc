.. http:get:: /payments/addr/(string:address)/

    Returns a list of payments sent to given address

    **Example request**:

    .. sourcecode:: python

        requests.get(
            "https://api.vigla.biz/payments/addr/75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa/",
            auth=HTTPBasicAuth(
                "56eDKfprZtQGfB4y6gVLZx5naKVHw6KEKLDoq2WWtLng9ANuBvsw67wfqyhQECoLmjQN4cKAdvMp2WsC5fnw9seKLcCSfjj",
                "b690d2d2-80cf-41f1-a2c8-29972c076d24"))

    **Example response**:

    .. sourcecode:: json

        [
          {
            "address": "75KLTYoKwsTFGBMSHg8AhD8MTxP4oz9JK2YgSF8RAhPsFgnkrJFbqEpRVdFwceyVtUhq1xHagUyqBAFEXJ4oBGRvDc54YXa",
            "amount": "3.141592653589",
            "height": 524845,
            "status": "mined",
            "last_status_notified": "mined",
            "txid": "f3a33600ec16c064765ad07644e458a4679e10fa9ca7acb3467a40ad3a639689"
          }
        ]

    :resheader Content-Type: application/json
    :statuscode 200: no error
    :statuscode 401: address and access token don't match
    :statuscode 404: the wallet doesn't contain specified subaddress
    :>json string address: the subaddress itself
    :>json string amount: the received amount formatted as string of ``.12f`` precision. A float is
            not used intentionally to avoid potential rounding errors.
    :>json int height: the height of the block containing the transaction, ``null`` if the
            transaction is still in mempool
    :>json string status: payment status, one of three values (``pool``, ``mined``, ``unlocked``)
    :>json string last_status_notified: last status for which a successful notification has been delivered
    :>json string txid: the ID (hash) of the transaction
