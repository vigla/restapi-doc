Notifications
=============

An important part of Vigla functionality are notifications. These are sent to your defined callback
``notification URL`` as soon as Vigla detects a new payment or a status change of existing one.

.. note:: Although Vigla accepts plain ``http`` notification URLs, it's strongly recommended that
        you provide a ``https`` endpoint.

Notification payload
--------------------

The callback carries JSON data of the received payment:

.. http:post:: <notification URL>

    **Example request**:

    .. sourcecode:: json

        {
          "amount": "1.234500000000",
          "height": null,
          "address": "78NjmbohsQNBJdJ7kyMBki4YMnHFAT91mX2jgGEEP2bEVmVYVjLwXBX9ZSMauGvijcUwAxGqxoBTa4Yq2MrwqdkR9Aswtku",
          "txid": "0c1d11bbf12b394fa832eb755fd189adb748c40cd46e04ba180ac390746d89b4",
          "signature": "sha256:08f96f3d613cf6b8bc24798eeb7d121f85ab3b5bc8441436339e4ad8741dae0d",
          "status": "pool",
          "confirmations": 0
        }

    :reqheader Content-Type: application/json
    :>json string amount: the received amount formatted as string of ``.12f`` precision. A float is
            not used intentionally to avoid potential rounding errors. The trailing zeroes are
            always included to make the representation uniform and easy to process in signature
            verification (see below).
    :>json int height: the height of the block containing the transaction, ``null`` if the
            transaction is still in mempool
    :>json string address: the destination address
    :>json string txid: the ID (hash) of the transaction
    :>json string signature: the signature of the notification
    :>json string status: payment status, one of three values (``pool``, ``mined``, ``unlocked``) described below
    :>json int confirmations: number of network confirmations (mined blocks) for the payment


Expected response
-----------------

The notification expects a ``HTTP 200`` response from the callback URL. If other status is
returned or connection fails, it will retry after about 2 seconds. Each subsequent failure
will double retry delay time until 15 attempts elapse. Then the notification will be dropped
as undeliverable.


Signature
---------

The signature is supplied to ensure the request comes from authentic Vigla software. Users
**should always validate the request**. The signature consists of two parts:

.. sourcecode::

    <hashing_algorithm>:<hash>

Currently only ``sha256`` algorithm is supported.

The ``hash`` is calculated by running the specified hash function on a colon-separated string
consisting of the most important parts of the information:

.. sourcecode::

    <amount>:<height>:<address>:<txid>:<access_token>

Where:
    * ``amount`` is the payment amount formatted exactly as received in the request (``.12f`` in *printf()* notation),
    * ``height`` is decimal height of the transaction's block or an empty string when height is ``null``,
    * ``address`` is the destination address,
    * ``txid`` is the hexadecimal transaction ID (hash),
    * ``access_token`` is the wallet access token in the UUID form.

.. warning:: **Never process data received from a notification with invalid checksum**.
    Please report such incidents to us (info@vigla.biz).


Payment statuses
----------------

There are three levels each payment may reach:

    * *pool*:

        The transaction is in the memory pool, a kind of storage where new transactions await being
        confirmed and registered on the blockchain. This level indicates a payment has been sent to
        you and validated by the network nodes to be properly constructed and spending funds the
        sender actually possesses.  However, in rare circumstances this may not be enough. There
        are potential attack scenarios, requiring some technical knowledge but little investment,
        where such transaction would get replaced and invalidated.

        Generally speaking, you may use this status to confirm orders where delivery speed is
        crucial and you may accept the risk of funds loss (although unlikely). A good scenario is
        coffee shop where drinks should be served ASAP but occasional loss of payment is not
        critical for the business.

    * *mined*:

        The transaction has been signed into a block and became a part of the blockchain. Usually
        it means the payment is secured but in a very unlikely event of `chain reorganization`_ the
        transaction may become invalidated and funds would return to the sender.

        Freshly mined funds are locked, which means it's impossible to spend them further. With
        each subsequent mined block the likelihood of reorganization shrinks drastically, making
        the transaction eventually reach the last status:

    * unlocked:

        Per standard of the Monero network, after 10 blocks have been mined, the funds become
        unlocked and the recipient may spend them in subsequent transaction. This is the last and
        most secure status of the payment.

        If the transaction has custom ``unlock_time`` set by the sender, this notification respects
        the setting and will arrive later, once the unlocking block has been mined.

.. _`chain reorganization`: https://learnmeabitcoin.com/guide/chain-reorganisation


Even though Vigla sends you notifications, you are free to :doc:`check status of your payments
<payment_queries>` at any given time.
