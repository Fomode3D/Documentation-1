***************
Getting Started
***************

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

   $  wget https://github.com/Remmeauth/remprotocol/releases/download/v0.1.0/remprotocol_0.1.0-ubuntu-18.04_amd64.deb && \
         sudo dpkg -i ./remnode_0.1.0-ubuntu-18.04_amd64.deb

Step 2: boot node and wallet
============================

Start keosd
-----------

Start ``keosd``:

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

Start nodeos
------------

Start ``nodeos``. This command loads all the basic plugins, set the server address, enable |cors_reference_mozzila|
(with no restrictions and development logging) and add some contract debugging and logging.

.. |cors_reference_mozzila| raw:: html

   <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS" target="_blank">CORS</a>

.. code-block:: console

   $ remnode -e -p rem \
         --plugin eosio::producer_plugin \
         --plugin eosio::chain_api_plugin \
         --plugin eosio::http_plugin \
         --access-control-allow-origin='*' \
         --contracts-console \
         --http-validate-host=false \
         --verbose-http-errors >> remnode.log 2>&1 &

.. note::

    In the above configuration, ``CORS`` is enabled for ``*`` for development purposes only, you should never enable
    ``CORS`` for ``*`` on a node that is publicly accessible!

Step 3: check that nodeos is producing blocks
=============================================

Run the following command:

.. code-block:: console

    tail -f remnode.log

You will see an output similar to the one below:

.. code-block:: console

    1929001ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366974ce4e2a... #13929 @ 2018-05-23T16:32:09.000 signed by eosio [trxs: 0, lib: 13928, confirmed: 0]
    1929502ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366aea085023... #13930 @ 2018-05-23T16:32:09.500 signed by eosio [trxs: 0, lib: 13929, confirmed: 0]
    1930002ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366b7f074fdd... #13931 @ 2018-05-23T16:32:10.000 signed by eosio [trxs: 0, lib: 13930, confirmed: 0]
    1930501ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366cd8222adb... #13932 @ 2018-05-23T16:32:10.500 signed by eosio [trxs: 0, lib: 13931, confirmed: 0]
    1931002ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366d5c1ec38d... #13933 @ 2018-05-23T16:32:11.000 signed by eosio [trxs: 0, lib: 13932, confirmed: 0]
    1931501ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366e45c1f235... #13934 @ 2018-05-23T16:32:11.500 signed by eosio [trxs: 0, lib: 13933, confirmed: 0]
    1932001ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000366f98adb324... #13935 @ 2018-05-23T16:32:12.000 signed by eosio [trxs: 0, lib: 13934, confirmed: 0]
    1932501ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 00003670a0f01daa... #13936 @ 2018-05-23T16:32:12.500 signed by eosio [trxs: 0, lib: 13935, confirmed: 0]
    1933001ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 00003671e8b36e1e... #13937 @ 2018-05-23T16:32:13.000 signed by eosio [trxs: 0, lib: 13936, confirmed: 0]
    1933501ms thread-0   producer_plugin.cpp:585       block_production_loo ] Produced block 0000367257fe1623... #13938 @ 2018-05-23T16:32:13.500 signed by eosio [trxs: 0, lib: 13937, confirmed: 0]

Press ``ctrl`` + ``c`` to close an output.





git clone -b block-producer-swap-bot --single-branch https://github.com/Remmeauth/remprotocol.git

cd remprotocol/block_producer_swap_bot

sudo ./install.sh


Step 3: check the wallet
========================

Run the following command, we need to validate the installation and check if wallet is working as intended:

.. code-block:: console

    $ remcli wallet list

You will see an output similar to the one below:

.. code-block:: console

    $ Wallets:
    []

Step 4: check nodeos endpoints
==============================

Run the following command, this will check that the ``RPC API`` is working correctly:

.. code-block:: console

   $ curl http://localhost:8888/v1/chain/get_info

Uninstall binaries
==================

Ubuntu
------

.. code-block:: console

   $ sudo dpkg -r remnode
