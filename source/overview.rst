Process overview
================

The example below describes a scenario where shop owner wants to accept Monero payments in their
store. The full wallet (also known as *spend wallet*) will be securely kept by the shop owner,
while the online store exposed to wild internets will contain not enough information for an
attacker to grab the collected funds.


Initial setup
-------------

 1. The shop owner creates a regular Monero wallet and keeps it in a safe place, writing
    down the ``address`` and ``secret view key``.
 2. He copies the ``notification URL`` from the shop. This is the address where e-commerce software
    expects notifications from Vigla.
 3. Using the *add wallet* option the shop owner submits these three pieces of information to
    Vigla.
 4. Vigla generates ``access token`` for the newly submitted wallet. The shop owner copies it
    back to the e-commerce software. The token is a kind of password that will be used in all
    communication between the shop and Vigla, to ensure both parties are authenticated.


Creating orders
---------------

 1. A customer orders at the shop and chooses Monero payment.
 2. The e-commerce software calls Vigla REST API to request a new ``address``, then stores it
    together with the order.
 3. The ``address`` is presented to the customer and he is requested to send the payment there.
 4. Once a payment appears, Vigla calls back the ``notification URL`` with information on how much
    has been received and what is the destination address.
 5. The e-commerce software checks cryptographic signature of the notification. It may also query
    Vigla API again to ensure the notification is authentic.
 6. Once the verification has succeeded, the e-commerce software marks the order as paid.
 7. The order may be delivered, meanwhile the funds have safely landed in shop owner's wallet.


Security and privacy implications
---------------------------------

 * Vigla asks for ``secret view key`` which is necessary to monitor the network for incoming
   transactions and to generate addresses belonging to the wallet. What Vigla operates is called
   a *view wallet* which is a read-only version that **cannot spend or otherwise affect collected
   funds**.
 * The shop is running without any wallet information, except the addresses used for existing
   orders. In case of data leak, the attacker will have no access to the wallet. He won't be able
   to spend funds. Once the ``access token`` is regenerated he won't be even able to monitor the
   network for further transactions.
   *(Of course any security breach would have much broader implications that are far out of scope
   of this document.)*
 * Should Vigla itself suffer a security breach and leak keys, the attacker would be able
   to access history of hosted wallets. Most of the privacy measures of Monero would be stripped
   down with following consequences:

        * The attacker would access full history of wallets hosted at Vigla, exposing trade volume
          and payment timestamps; wallets' owners would end up in a situation similar to what
          they would start with if using an open-ledger cryptocurrency (like Bitcoin).
        * The identity of payment senders would not be revealed.
        * No funds would be accessed by the attacker.

Having learned those basics, you may now quickly :doc:`set up the API connection <quickstart>`.
