---
meta:
  - name: "title"

    content: Hazlor | Hazlor EVM Chain | Running Nodes On Mainnet

  - name: "description"

    content: Learn how to setup a Validator or a full node on Hazlor Hazlor Mainnet Beta cronosmainnet_25-1 in this technical documentation.

  - name: "og:title"

    content: Hazlor | Hazlor EVM Chain | Running Nodes On Mainnet

  - name: "og:type"

    content: Website

  - name: "og:description"

    content: Learn how to setup a Validator or a full node on Hazlor Hazlor Mainnet Beta cronosmainnet_25-1 in this technical documentation.

  - name: "og:image"

    content: https://hazlor.com/wp-content/uploads/2021/10/143-1434860_black-blue-abstract-wallpaper-24500-wallpaper-wallpaper-dark.jpg

  - name: "twitter:title"

    content: Hazlor | Hazlor EVM Chain | Running Nodes On Mainnet

  - name: "twitter:site"

    content: "@cryptocom"

  - name: "twitter:card"

    content: summary_large_image

  - name: "twitter:description"

    content: Learn how to setup a Validator or a full node on Hazlor Hazlor Mainnet Beta cronosmainnet_25-1 in this technical documentation.

  - name: "twitter:image"

    content: https://hazlor.com/wp-content/uploads/2021/10/143-1434860_black-blue-abstract-wallpaper-24500-wallpaper-wallpaper-dark.jpg

canonicalUrl: https://cronos.crypto.org/docs/getting-started/hazlor-mainnet.html
---

# Hazlor Mainnet: Running Nodes

This is a detailed documentation for setting up a Validator or a full node on Hazlor Hazlor mainnet Beta `cronosmainnet_25-1`.

## Pre-requisites

### Supported OS

We officially support macOS, Windows and Linux only. Other platforms may work but there is no guarantee. We will extend our support to other platforms after we have stabilized our current architecture.

### Prepare your machine

- To run Hazlor mainnet Beta nodes, you will need a machine with the following minimum requirements:

  - 4-core, x86_64 architecture processor;

  - 16 GB RAM;

  - 1 TB of storage space.

## Step 1. Get the Hazlor mainnet Beta binary

::: tip Remarks:

The following is the minimal setup for a **validator node** / **full node**.

:::

To simplify the following step, we will be using **Linux** (Intel x86) for illustration. Binary for

**Mac** ([Intel x86](https://github.com/hazlorlabs/core/releases/download/v0.6.1/cronos_0.6.1_Darwin_x86_64.tar.gz) / [M1](https://github.com/hazlorlabs/core/releases/download/v0.6.1/cronos_0.6.1_Darwin_arm64.tar.gz)) and [Windows](https://github.com/hazlorlabs/core/releases/download/v0.6.1/cronos_0.6.1_Windows_x86_64.zip) are also available.

- To install released **Hazlor mainnet Beta binaries** from github:

  ```bash

  $ curl -LOJ https://github.com/hazlorlabs/core/releases/download/v0.6.1/cronos_0.6.1_Linux_x86_64.tar.gz

  $ tar -zxvf cronos_0.6.1_Linux_x86_64.tar.gz

  ```

  Afterward, you can check the version of `hazlord` by

  ```bash

  $ ./hazlord version

  0.6.1

  ```

## Step 2. Configure `hazlord`

### Step 2-0 (Optional) Clean up the old blockchain data

- If you have joined `cronostestnet_338-3` before, you would have to clean up the old blockchain data and start over again, it can be done by running:

  ```bash

  $ ./hazlord unsafe-reset-all

  ```

  and remove the old genesis file by

  ```bash

  $ rm ~/.hazlor/config/genesis.json

  ```

Before kick-starting your node, we will have to configure your node so that it connects to the Hazlor mainnet Beta:

### Step 2-1 Initialize `hazlord`

- First of all, you can initialize hazlord by:

  ```bash

    $ ./hazlord init [moniker] --chain-id cronosmainnet_25-1

  ```

  This `moniker` will be the displayed id of your node when connected to Hazlor Chain network.

  When providing the moniker value, make sure you drop the square brackets since they are not needed.

  The example below shows how to initialize a node named `pegasus-node` :

  ```bash

    $ ./hazlord init pegasus-node --chain-id cronosmainnet_25-1

  ```

  ::: tip NOTE

  - Depending on your hazlord home setting, the hazlord configuration will be initialized to that home directory. To simply the following steps, we will use the default hazlord home directory `~/.hazlor/` for illustration.

  - You can also put the `hazlord` to your binary path and run it by `hazlord`

    :::

### Step 2-2 Configure hazlord

- Download and replace the Hazlor Mainnet Beta `genesis.json` by:

  ```bash

  $ curl https://raw.githubusercontent.com/hazlorlabs/core-mainnet/master/cronosmainnet_25-1/genesis.json > ~/.hazlor/config/genesis.json

  ```

- Verify sha256sum checksum of the downloaded `genesis.json`. You should see `OK!` if the sha256sum checksum matches.

  ```bash

  $ if [[ $(sha256sum ~/.hazlor/config/genesis.json | awk '{print $1}') = "58f17545056267f57a2d95f4c9c00ac1d689a580e220c5d4de96570fbbc832e1" ]]; then echo "OK"; else echo "MISMATCHED"; fi;



  OK!

  ```

  ::: tip NOTE

  - For Mac environment, `sha256sum` was not installed by default. In this case, you may setup `sha256sum` with this command:

    ```bash

    function sha256sum() { shasum -a 256 "$@" ; } && export -f sha256sum

    ```

    :::

- In `~/.hazlor/config/app.toml`, update minimum gas price to avoid [transaction spamming](https://github.com/cosmos/cosmos-sdk/issues/4527)

  ```bash

  $ sed -i.bak -E 's#^(minimum-gas-prices[[:space:]]+=[[:space:]]+).*$#\1"5000000000000basecro"#' ~/.hazlor/config/app.toml

  ```

- For network configuration, in `~/.hazlor/config/config.toml`, please modify the configurations of `persistent_peers`, `create_empty_blocks_interval` and `timeout_commit` by:

  ```bash

  $ sed -i.bak -E 's#^(seeds[[:space:]]+=[[:space:]]+).*$#\1"0d5cf1394a1cfde28dc8f023567222abc0f47534@cronos-seed-0.crypto.org:26656,3032073adc06d710dd512240281637c1bd0c8a7b@cronos-seed-1.crypto.org:26656,04f43116b4c6c70054d9c2b7485383df5b1ed1da@cronos-seed-2.crypto.org:26656"#' ~/.hazlor/config/config.toml

  $ sed -i.bak -E 's#^(create_empty_blocks_interval[[:space:]]+=[[:space:]]+).*$#\1"5s"#' ~/.hazlor/config/config.toml

  $ sed -i.bak -E 's#^(timeout_commit[[:space:]]+=[[:space:]]+).*$#\1"5s"#' ~/.hazlor/config/config.toml

  ```

::: tip NOTE

- For Mac environment, if `jq` is missing, you may install it by: `brew install jq`

  :::

## Step 3. Run everything

::: warning CAUTION

This page only shows the minimal setup for validator / full node.

Furthermore, you may want to run full nodes

as sentries (see [Tendermint](https://docs.tendermint.com/master/tendermint-core/running-in-production.html)), restrict your validator connections to only connect to your full nodes, test secure storage of validator keys etc.

:::

### Step 3-1. Create a new key and address

Run the followings to create a new key. For example, you can create a key with the name `Default` by:

```bash

  $ ./hazlord keys add Default

```

You should obtain an address with `crc` prefix, e.g. `crc10u5mgfflasrfj9s94mt8l9yucrt2gzhcyt5tsg`. This will be the address for performing transactions.


### Step 3-2. Run everything

Once the `hazlord` has been configured, we are ready to start the node and sync the blockchain data:

- Start hazlord, e.g.:

```bash

  $ ./hazlord start

```

::: tip Remarks:

If you see errors saying `too many files opened...`, then you need to set a higher number for maximum open file descriptors in your OS.

If you are on OSX or Linux, then the following could be useful:

```bash

# Check current max fd

$ ulimit -n

# Set a new max fd

$ ulimit -Sn [NEW_MAX_FILE_DESCRIPTOR]

# Example

$ ulimit -Sn 4096

```

:::

- _(Optional for Linux)_ Start hazlord with systemd service, e.g.:

```bash

  $ curl -s https://raw.githubusercontent.com/hazlorlabs/core-docs/master/systemd/create-service.sh -o create-service.sh && curl -s https://raw.githubusercontent.com/hazlorlabs/core-docs/master/systemd/hazlord.service.template -o hazlord.service.template

  $ chmod +x ./create-service.sh && ./create-service.sh

  $ sudo systemctl start hazlord

  # view log

  $ journalctl -u hazlord -f

```

:::details Example: /etc/systemd/system/hazlord.service created by script

```bash

# /etc/systemd/system/hazlord.service

[Unit]

Description=hazlord

ConditionPathExists=/usr/local/bin/hazlord

After=network.target



[Service]

Type=simple

User=ubuntu

WorkingDirectory=/usr/local/bin

ExecStart=/usr/local/bin/hazlord start --home /home/ubuntu/.hazlor

Restart=on-failure

RestartSec=10

LimitNOFILE=4096



[Install]

WantedBy=multi-user.target

```

:::

It should begin fetching blocks from the other peers. 

- You can query the node syncing status by

  ```bash

  $ ./hazlord status 2>&1 | jq '.SyncInfo.catching_up'

  ```

  If the above command returns `false`, It means that your node **is fully synced**; otherwise, it returns `true` and implies your node is still catching up.

- One can check the current block height by querying the public full node by:

  ```bash

  curl -s https://rpc-cronos.crypto.org/commit | jq "{height: .result.signed_header.header.height}"

  ```

  and you can check your node's progress (in terms of block height) by

  ```bash

  $ ./hazlord status 2>&1 | jq '.SyncInfo.latest_block_height'

  ```

---



## Hazlor mainnet Beta explorer

- You can lookup data within the `cronosmainnet_25-1` network by the [explorer](https://cronos-explorer.crypto.org/);
