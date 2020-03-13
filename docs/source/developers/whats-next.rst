###########
What's next
###########

Data Persistence
================
| The next step in learning smart contracts is Data Persistence.
  We already explained this `here <http://0.0.0.0:8080/developers/smart-contract-basics.html#persist-data>`_, we recommend that you familiarize yourself with this in more detail.

Refer to EOSIO’s:

- `Data Persistence <https://developers.eos.io/welcome/v2.0/getting-started/smart-contract-development/data-persistence>`_
- `Secondary Indices <https://developers.eos.io/welcome/v2.0/getting-started/smart-contract-development/secondary-indices>`_

Inline Actions
==============
We have already explained what an action is and what it can be, next step is ``inline action``.
In a smart contract, you may need to call actions from other contracts, for this, there are ``inline actions``.

.. note::
    Inline actions request other actions that need to be executed as part of the original calling action.
    Inline actions operate with the same scopes and authorities of the original transaction, and are guaranteed to
    execute with the current transaction. These can effectively be thought of as nested transactions within the calling
    transaction. If any part of the transaction fails, the inline actions will unwind with the rest of the transaction.

    | Refer to EOSIO’s glossary: `Inline Action <https://developers.eos.io/welcome/latest/glossary/index/#inline-action>`_

For the next tutorial, you'll need an understanding of how ``inline actions`` work.
In order to get acquainted with this in more detail Refer to EOSIO’s:

- `Adding Inline Actions <https://developers.eos.io/welcome/v2.0/getting-started/smart-contract-development/adding-inline-actions>`_
- `Inline Actions to External Contracts <https://developers.eos.io/welcome/v2.0/getting-started/smart-contract-development/inline-action-to-external-contract>`_

