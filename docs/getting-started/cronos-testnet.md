---
meta:
  - name: "title"
    content: Hazlor | Hazlor EVM Chain | Running Nodes On Testnet
  - name: "description"
    content: Learn how to setup a Validator or a full node on Hazlor Hazlor testnet hazlor_7878-1 in this technical documentation.
  - name: "og:title"
    content: Hazlor | Hazlor EVM Chain | Running Nodes On Testnet
  - name: "og:type"
    content: Website
  - name: "og:description"
    content: Learn how to setup a Validator or a full node on Hazlor Hazlor testnet hazlor_7878-1 in this technical documentation.
  - name: "og:image"
    content: https://hazlor.com/wp-content/uploads/2021/10/143-1434860_black-blue-abstract-wallpaper-24500-wallpaper-wallpaper-dark.jpg
  - name: "twitter:title"
    content: Hazlor | Hazlor EVM Chain | Running Nodes On Testnet
  - name: "twitter:site"
    content: "@cryptocom"
  - name: "twitter:card"
    content: summary_large_image
  - name: "twitter:description"
    content: Learn how to setup a Validator or a full node on Hazlor Hazlor testnet hazlor_7878-1 in this technical documentation.
  - name: "twitter:image"
    content: https://hazlor.com/wp-content/uploads/2021/10/143-1434860_black-blue-abstract-wallpaper-24500-wallpaper-wallpaper-dark.jpg
canonicalUrl: https://docs.hazlor.com/docs/getting-started/cronos-testnet.html
---

# Hazlor Testnet: Running Nodes

The latest Hazlor Chain Testnet has been named as **Hazlor**.

This is a detailed documentation for setting up a Validator or a full node on Hazlor Hazlor testnet `hazlor_7878-1`.

## Pre-requisites

### Supported OS

We officially support macOS, Windows and Linux only. Other platforms may work but there is no guarantee. We will extend our support to other platforms after we have stabilized our current architecture.

### Prepare your machine

- To run Hazlor testnet nodes, you will need a machine with the following minimum requirements:

  - 4-core, x86_64 architecture processor;
  - 16 GB RAM;
  - 1 TB of storage space.

## Step 1. Get the Hazlor testnet binary

::: tip Remarks:
The following is the minimal setup for a **validator node** / **full node**.
:::

To simplify the following step, we will be using **Linux** (Intel x86) for illustration. Binary for
**Mac** ([Intel x86](https://github.com/hazlorlabs/core/releases/download/v0.5.5-testnet/cronos_0.5.5-testnet_Darwin_x86_64.tar.gz) / [M1](https://github.com/hazlorlabs/core/releases/download/v0.5.5-testnet/cronos_0.5.5-testnet_Darwin_arm64.tar.gz))and [Windows](https://github.com/hazlorlabs/core/releases/download/v0.5.5-testnet/cronos_0.5.5-testnet_Windows_x86_64.zip) are also available.

- To install released **Hazlor testnet binaries** from github:

  ```bash
  $ curl -LOJ https://github.com/hazlorlabs/core/releases/download/v0.6.0-testnet/cronos_0.6.0-testnet_Linux_x86_64.tar.gz
  $ tar -zxvf cronos_0.6.0-testnet_Linux_x86_64.tar.gz
  ```

  Afterward, you can check the version of `hazlord` by

  ```bash
  $ hazlord version
  0.6.0-testnet
  ```

## Step 2. Configure `hazlord`


### Step 2-0 (Optional) Clean up the old blockchain data

- If you have joined `cronostestnet_338-2` before, you would have to clean up the old blockchain data and start over again, it can be done by running:

  ```bash
  $ hazlord unsafe-reset-all
  ```
  and remove the old genesis file by

  ```bash
  $ rm ~/.hazlor/config/genesis.json
  ```

Before kick-starting your node, we will have to configure your node so that it connects to the Hazlor testnet:

### Step 2-1 Initialize `hazlord`

- First of all, you can initialize hazlord by:


  ```bash
    $ hazlord init [moniker] --chain-id hazlor_7878-1
  ```



  This `moniker` will be the displayed id of your node when connected to Hazlor Chain network.
  When providing the moniker value, make sure you drop the square brackets since they are not needed.
  The example below shows how to initialize a node named `pegasus-node` :


  ```bash
    $ hazlord init pegasus-node --chain-id hazlor_7878-1
  ```



  ::: tip NOTE

  - Depending on your hazlord home setting, the hazlord configuration will be initialized to that home directory. To simply the following steps, we will use the default hazlord home directory `~/.hazlor/` for illustration.
  - You can also put the `hazlord` to your binary path and run it by `hazlord`
    :::

### Step 2-2 Configure hazlord



- Download and replace the Hazlor Testnet `genesis.json` by:

  ```bash
  $ curl https://raw.githubusercontent.com/hazlorlabs/core-testnets/main/hazlor_7878-1/genesis.json > ~/.hazlor/config/genesis.json
  ```


- Verify sha256sum checksum of the downloaded `genesis.json`. You should see `OK!` if the sha256sum checksum matches.


  ```bash
  $ if [[ $(sha256sum ~/.hazlor/config/genesis.json | awk '{print $1}') = "7d898ad75b3e2e1fa182d928ca10a284c1dd252e12d17ad6dab76551b29d1a59" ]]; then echo "OK"; else echo "MISMATCHED"; fi;

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
  $ sed -i.bak -E 's#^(minimum-gas-prices[[:space:]]+=[[:space:]]+).*$#\1"5000000000000basetscas"#' ~/.hazlor/config/app.toml
  ```

- For network configuration, in `~/.hazlor/config/config.toml`, please modify the configurations of `persistent_peers`, `create_empty_blocks_interval` and `timeout_commit` by:
  ```bash
  $ sed -i.bak -E 's#^(persistent_peers[[:space:]]+=[[:space:]]+).*$#\1"7af13cfce2eda89cc883188ce34b6608a710ede0@143.110.159.76:26656"#' ~/.hazlor/config/config.toml
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
  $ hazlord keys add Default
```

You should obtain an address with `tscas` prefix, e.g. `tscas10u5mgfflasrfj9s94mt8l9yucrt2gzhcyt5tsg`. This will be the address for performing transactions.

### Step 3-2. Obtain test token

Users can the [faucet](https://faucet.hazlor.com) to obtain test tokens, please note that you would need a Ethereum type address `0x...` that can be obtained by

- [Using metamask](./metamask.md#using-metamask); or
- Using the [address convention tool](./metamask.md#address-conventions).

In case you have reached the daily limit on faucet , you can simply send a message on [Discord](https://discord.gg/pahqHz26q4) #request-tscas channel ,
stating who you are and your `0x...` address.

### Step 3-3. Run everything

Once the `hazlord` has been configured, we are ready to start the node and sync the blockchain data:

- Start hazlord, e.g.:

```bash
  $ hazlord start
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

It should begin fetching blocks from the other peers. Please wait until it is fully synced before moving onto the next step.

- You can query the node syncing status by

  ```bash
  $ hazlord status 2>&1 | jq '.SyncInfo.catching_up'
  ```

  If the above command returns `false`, It means that your node **is fully synced**; otherwise, it returns `true` and implies your node is still catching up.

- One can check the current block height by querying the public full node by:

  ```bash
  curl -s https://hatlas.rpc.hazlor.com:26657/commit | jq "{height: .result.signed_header.header.height}"
  ```

  and you can check your node's progress (in terms of block height) by

  ```bash
  $ hazlord status 2>&1 | jq '.SyncInfo.latest_block_height'
  ```

The next step only applied to the validators, in which, validator hosting is by invitation only at the early stage of the Hazlor testnet.

---

### Step 3-4. Obtain the validator public key

You can obtain your validator public key by:

```bash
  $ hazlord tendermint show-validator
```

The public key should in a json format, for example:

```json
{
  "@type": "/cosmos.crypto.ed25519.PubKey",
  "key": "gvPPVShkWjuUn7cuqS3ci9fHnC+nLFxzsNWkwGJ6iMI="
}
```

### Step 3-5. Send a `create-validator` transaction

Once the node is fully synced, we are now ready to send a `create-validator` transaction and join the network, for example:

```
$ hazlord tx staking create-validator \
--from=[name_of_your_key] \
--amount=10000000stake \
--pubkey='{"@type":"/cosmos.crypto.ed25519.PubKey","key":"PUBLIC_KEY"}'  \
--moniker="[The_id_of_your_node]" \
--security-contact="[security contact email/contact method]" \
--chain-id="hazlor_7878-1" \
--commission-rate="0.10" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.01" \
--min-self-delegation="1" \
--fees=1000000000000000000basetscas

{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgCreateValidator"...}
confirm transaction before signing and broadcasting [y/N]: y
```

You will be required to insert the following:


- `--from`: The `tscas...` address that holds your `stake` token;
- `--pubkey`: The validator public key( See Step [3-4](#step-3-4-obtain-the-validator-public-key) above )
- `--moniker`: A moniker (name) for your validator node;
- `--security-contact`: Security contact email/contact method.

### Step 3-6. Check your validator status

Once the `create-validator` transaction completes, you can check if your validator has been added to the validator set:

```bash

$ hazlord tendermint show-validator
## [{"@type":"/cosmos.crypto.ed25519.PubKey","key":"VALIDATOR_KEY"}] ##
$ hazlord query tendermint-validator-set | grep -c [VALIDATOR_KEY]

## 1 = Yes; 0 = Not yet added ##
```

To further check if the validator is signing blocks, kindly run this [script](https://github.com/crypto-com/chain-docs/blob/master/docs/getting-started/assets/signature_checking/check-validator-up.sh), for example:

```bash
$ curl -sSL https://raw.githubusercontent.com/crypto-com/chain-docs/master/docs/getting-started/assets/signature_checking/check-validator-up.sh | bash -s -- \
--tendermint-url https://hatlas.rpc.hazlor.com:26657 \
--pubkey $(cat ~/.hazlor/config/priv_validator_key.json | jq -r '.pub_key.value')

The validator is in the active validator set under the address  <YOUR_VALIDATOR_ADDRESS>
The validator is signing @ Block#<BLOCK_HEIGHT> ????
```


## Step 4. Perform Transactions

### Step 4-1. `query bank balances` - Check your transferable balance

You can check your _transferable_ balance with the `balances` command under the bank module.
:::details Example: Check your address balance

```bash
$ hazlord query bank balances tscas1qsklxwt77qrxur494uvw07zjynu03dq9alwh37

balances:
- amount: "10005471622381693"
  denom: basetscas
pagination:
  next_key: null
  total: "0"

```

:::



### Slashing related transaction

### `tx slashing unjail` - Unjail a validator

Validator could be punished and jailed due to network misbehaviour, we can check the jailing status of a validator, for example:

```bash
$ hazlord query staking validators -o json | jq
................................
      "operator_address": "tscasvaloper1hhskvvt87ngxjgl4fkcrn3ts09u63pnh47t06u",
      "consensus_pubkey": {
        "@type": "/cosmos.crypto.ed25519.PubKey",
        "key": "rXphE0lECaU4MfBL70l6tGrfaply4dp79g7ql4ijfco="
      },
      "jailed": false,
................................
```

Where `"jailed": true` implies that the validator has been jailed. After the jailing period has passed, one can broadcast a `unjail` transaction to unjail the validator and resume its normal operations by

```bash
$ hazlord tx slashing unjail --from [key_name] --chain-id "hazlor_7878-1" --fees=1000000000000000000basetscas

  {"body":{"messages":[{"@type":"/cosmos.slashing.v1beta1.MsgUnjail"...}]}
  confirm transaction before signing and broadcasting [y/N]: y
```

:::

Congratulations! You've successfully set up a Testnet node and performed some basic transactions! You may refer to [Wallet Management](https://docs.hazlor.com/docs/wallets/cli.html#hazlord) for more advanced operations and transactions.

## Hazlor testnet faucet and explorer



- You can lookup data within the `hazlor_7878-1` network by the [explorer](https://explorer.hazlor.com);

- To interact with the blockchain, simply use the [test-token faucet](https://faucet.hazlor.com) to obtain test CRO tokens for performing transactions on the **Hazlor** testnet.

  - Note that you will need to create an [address](#step-3-1-create-a-new-key-and-address) before using the faucet.

