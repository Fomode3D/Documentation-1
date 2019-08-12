*********
Testchain
*********

Disclaimer
==========

There are a variety of tools which require to have basic knowledge on how to use them in the terminal.

Step 1: install binaries
========================

If you have previous versions of ``Remme protocol`` installed on your machine, please uninstall before proceeding.
Detailed instructions are below.

Ubuntu 18.04
------------

.. code-block:: console

   $ wget https://github.com/Remmeauth/remprotocol/releases/download/v0.1.0/remmeprotocol_0.1.0-ubuntu-18.04_amd64.deb && \
         sudo dpkg -i ./remmeprotocol_0.1.0-ubuntu-18.04_amd64.deb

Step 2: boot node and wallet
============================

Start remvault
--------------

Start ``remvault``:

.. code-block:: console

   $ remvault &

You will see an output similar to the one below:

.. code-block:: console

   info  2019-08-12T13:16:38.388 remvault  http_plugin.cpp:625           add_handler          ] add api url: /v1/remvault/stop
   info  2019-08-12T13:16:38.389 remvault  http_plugin.cpp:625           add_handler          ] add api url: /v1/node/get_supported_apis
   info  2019-08-12T13:16:38.389 remvault  wallet_api_plugin.cpp:73      plugin_startup       ] starting wallet_api_plugin
   info  2019-08-12T13:16:38.389 remvault  http_plugin.cpp:625           add_handler          ] add api url: /v1/wallet/create
   info  2019-08-12T13:16:38.389 remvault  http_plugin.cpp:625           add_handler          ] add api url: /v1/wallet/create_key
   info  2019-08-12T13:16:38.389 remvault  http_plugin.cpp:625           add_handler          ] add api url: /v1/wallet/get_public_keys

Press enter to continue.

Step 3: download testchain settings
==================================

.. code-block:: console

   $ wget https://testchain.remme.io/genesis.json

Step 4: create configuration file
=================================

Create ``data`` and ``config`` folders.

.. code-block:: console

   $ mkdir data && mkdir config

Create ``config/config.ini`` and put the following settings into it:

.. code-block:: console

   plugin = eosio::chain_api_plugin
   plugin = eosio::net_api_plugin
   http-server-address = 0.0.0.0:8888
   p2p-listen-endpoint = 0.0.0.0:9876
   p2p-peer-address = 167.71.88.152:9877
   verbose-http-errors = true

These config options should get you into the basic operation mode with your node API available at port ``8888``. P2p-peer-address
points to the other nodes where to fetch the new blocks from (you may specify multiple entries, ``167.71.88.152`` is the address of
a node hosted by ``Remme``).

Start remnode
-------------

Start ``remnode``.

.. code-block:: console

   $ remnode --config-dir ./config/ --data-dir ./data/ --delete-all-blocks --genesis-json genesis.json

.. code-block:: console

   $ remnode --config-dir ./config/ --data-dir ./data/ >> remnode.log 2>&1 &

The command above will run the node in the background and will save its output to the ``remnode.log`` file. At this point,
you must be ready to start and connect your node to the network. If your node is connected and synced, this command
should return you the information about the chain:

.. code-block:: console

   $ remcli get info
     {
         "server_version": "96796929",
         "chain_id": "93ece941df27a5787a405383a66a7c26d04e80182adf504365710331ac0625a7",
         "head_block_num": 680455,
         "last_irreversible_block_num": 680121,
         "last_irreversible_block_id": "000a60b93d787895c905e36d7cf8d37a2bbed21d6f4b04f55645aefe459a32c0",
         "head_block_id": "000a62074d3b6919262d90beecdffcc021fca03dc9ecd01ce4bfb91f8af36720",
         "head_block_time": "2019-08-12T15:08:58.500",
         "head_block_producer": "remproduce21",
         ...
     }

``remcli`` (analog of cleos in EOSIO terms) is a command-line tool that has a rich variety of functions. It has
nearly everything that you may need to interact with the blockchain. You may
start getting familiar with it by running ``remcli --help``.

Step 5: become a block producer
===============================

To become a block producer you need to register your account via a system smart contract by calling the action ``regproducer``,
vote for someone or yourself, set up your node as a full node (described above) and prepare it for block production (so it starts to
produce blocks in case you make it to the ``top21``).

.. code-block:: console

   $ remcli system regproducer YOURACCOUNTNAME YOURPUBLICKEY https://yourdomain.com
   $ remcli system voteproducer prods YOURACCOUNTNAME YOURACCOUNTNAME

In your node config file, add these options:

.. code-block:: console

   plugin = eosio::producer_plugin
   plugin = eosio::producer_api_plugin
   producer-name = YOURACCOUNTNAME
   signature-provider = YOURPUBLICKEY=KEY:YOURPRIVATEKEY

Once you run remnode, these config options should get you into block producer operation mode with your node. Once your block producer
account gets into the top21 list, your node will automatically start producing blocks. Please pay attention that on the contrary
to ``EOS`` network, ``Block Producers`` on ``testchain`` are required to validate the token swaps between the chains and have to
run an additional bot (along the ``remnode``) that monitors external blockchains (e.g. ``Ethereum``).

Found installation guide for ``Token swap`` below.

Token swap
----------

Download sources:

.. code-block:: console

   $ git clone -b block-producer-swap-bot --single-branch https://github.com/Remmeauth/remprotocol.git && \
         cd remprotocol/block_producer_swap_bot

If you use ``Ubuntu 16.04``, install dependencies with the following command:

.. code-block:: console

   $ sudo ./scripts/ubuntu16.04_install.sh

If you use ``Ubuntu 18.04``, install dependencies with the following command

.. code-block:: console

   $ sudo ./scripts/ubuntu18.04_install.sh

Create configuration file with the following command:

.. code-block:: console

   $ nano ./config.ini

Paste into the config file the following content:

.. code-block:: console

   [NODES]
   remnode=127.0.0.1:8888
   eth-provider=wss://ropsten.infura.io/ws/v3/<your infura id>
   [REM]
   swap-permission=<permission to authorize init swap actions>@active
   swap-private-key=<private key to sign init swap actions>

Replace ``remnode``, ``eth-provider``, ``swap-permission``, ``swap-private-key`` with your remnode host and port, a link to
``Ethereum`` node with websocket connection, your account and private key to authorize init swap actions (for example your block producer
account name and private key for signing blocks). |infura_api_key_reference|.

.. |infura_api_key_reference| raw:: html

   <a href="https://ethereumico.io/knowledge-base/infura-api-key-guide" target="_blank">Tutorial for creating Infura API key</a>

Save config file with ``Ctrl+O``. Press ``Enter``. Close config file with ``Ctrl+X``.

To start approving swaps run the following command:

.. code-block:: console

   $ sudo ./scripts/run.sh >> swap.log 2>&1 &
