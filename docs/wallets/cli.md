---
meta:
  - name: "title"
    content: Hazlor | hazlord
  - name: "description"
    content: hazlord is the all-in-one command-line interface. It supports wallet management, funds transfer and staking operations.
  - name: "og:title"
    content: Hazlor | hazlord
  - name: "og:type"
    content: Website
  - name: "og:description"
    content: hazlord is the all-in-one command-line interface. It supports wallet management, funds transfer and staking operations.
  - name: "og:image"
    content: https://cronos.crypto.org/og-image.png
  - name: "twitter:title"
    content: Hazlor | hazlord
  - name: "twitter:site"
    content: "@cryptocom"
  - name: "twitter:card"
    content: summary_large_image
  - name: "twitter:description"
    content: hazlord is the all-in-one command-line interface. It supports wallet management, funds transfer and staking operations.
  - name: "twitter:image"
    content: https://cronos.crypto.org/og-image.png
canonicalUrl: https://cronos.crypto.org/docs/wallets/cli.html
---

# hazlord

`hazlord` is the all-in-one command-line interface. It supports wallet management, funds transfer and staking operations.

## Build and configurations

### Build Prerequisites

- You can get the latest `hazlord` binary here from the [release page](https://github.com/hazlorlabs/core/releases);

### Using `hazlord`

`hazlord`is bundled with the Hazlor Chain code. After you have obtained the latest `hazlord` binary, run

```bash
$ hazlord [command]
```

There is also a `-h, --help` command available

```bash
$ hazlord -h
```

### Config and data directory

By default, your config and data are stored in the folder located at the `~/.hazlor` directory.

Make sure you have backed up your wallet storage after creating the wallet or else your funds may be inaccessible in case of accident forever.

#### Configure hazlord config and data directory

To specify the hazlord config and data storage directory; you can add a global flag `--home <directory>`

## Configuration Setting

We can view the default config setting by using `hazlord config` command:

```
$ hazlord config
{
	"chain-id": "",
	"keyring-backend": "os",
	"output": "text",
	"node": "tcp://localhost:26657",
	"broadcast-mode": "sync"
}
```

We can make changes to the default settings upon our choices, so it allows users to set the configuration beforehand all at once, so it would be ready with the same config afterward.

For example, the `chain-id` can be changed to `cronostestnet_338-1` from a blank name by

```
$ hazlord config "chain-id" cronostestnet_338-1
$ hazlord config
{
	"chain-id": "cronostestnet_338-1",
	"keyring-backend": "os",
	"output": "text",
	"node": "tcp://localhost:26657",
	"broadcast-mode": "sync"
}
```

Other values can be changed in the same way.

Alternatively, we can directly make the changes to the config values in one place at client.toml. It is under the path of `.ethermint/config/client.toml` in the folder where we installed ethermint:

```
############################################################################
###                         Client Configuration                         ###
############################################################################

# The network chain ID
chain-id = "cronostestnet_338-1"
# The keyring's backend, where the keys are stored (os|file|kwallet|pass|test|memory)
keyring-backend = "os"
# CLI output format (text|json)
output = "number"
# <host>:<port> to Tendermint RPC interface for this chain
node = "tcp://localhost:26657"
# Transaction broadcasting mode (sync|async|block)
broadcast-mode = "sync"
```

After the necessary changes are made in the `client.toml`, then save.
For example, if we directly change the `chain-id` from `ethermint0` to ethermint-test1, and output to number, it would change instantly as shown below.

```
$ hazlord config
{
	"chain-id": "ethermint-test1",
	"keyring-backend": "os",
	"output": "number",
	"node": "tcp://localhost:26657",
	"broadcast-mode": "sync"
}
```

### Options

A list of commonly used flags of hazlord is listed below:

| Option              | Description                   | Type         | Default Value   |
| ------------------- | ----------------------------- | ------------ | --------------- |
| `--home`            | Directory for config and data | string       | `~/.hazlor` |
| `--chain-id`        | Full Chain ID                 | String       | ---             |
| `--output`          | Output format                 | string       | "text"          |
| `--keyring-backend` | Select keyring's backend      | os/file/test | os              |

## Command list

A list of commonly used `hazlord` commands.

| Command | Description                                                         | List                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| ------- | ------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `keys`  | [Keys management](#keys-management-hazlord-keys)                 | [`add <wallet_name>`](#keys-add-wallet-name-create-a-new-key)<br /><br />[`add <key_name> --recover`](#keys-add-key-name-recover-restore-existing-key-by-seed-phrase)<br /><br />[`list`](#keys-list-list-your-keys)<br /><br />[`show <key_name>`](#keys-show-key-name-retrieve-key-information)<br /><br />[`delete <key_name>`](#keys-delete-key-name-delete-a-key)<br /><br />[`export <key_name>`](#keys-export-key-name-export-private-keys) |
| `tx`    | [Transactions subcommands](#transactions-subcommands-hazlord-tx) | [`bank send`](#tx-bank-send-transfer-operation)<br /><br />[`staking delegate`](#delegate-you-funds-to-a-validator-tx-staking-delegate-validator-addr-amount)<br /><br />[`staking unbond`](#unbond-your-delegated-funds-tx-staking-unbond-validator-addr-amount)<br /><br />[`staking create-validator`](#tx-staking-create-validator-joining-the-network-as-a-validator)<br /><br />[`slashing unjail`](#tx-slashing-unjail-unjail-a-validator)  |
| `query` | [Query subcommands](#balance-transaction-history)                   | [`query bank balance`](#query-bank-balances-check-your-transferable-balance)                                                                                                                                                                                                                                                                                                                                                                       |

You may also add the flag `-h, --help` on `hazlord [command]` to get more available commands and details.

::: details Example: More details of subcommand - tx staking

```bash
$ hazlord tx staking --help
Staking transaction subcommands

Usage:
  hazlord tx staking [flags]
  hazlord tx staking [command]

Available Commands:
  create-validator create new validator initialized with a self-delegation to it
  delegate         Delegate liquid tokens to a validator
  edit-validator   edit an existing validator account
  redelegate       Redelegate illiquid tokens from one validator to another
  unbond           Unbond shares from a validator

Flags:
  -h, --help   help for staking

Global Flags:
      --chain-id string     The network chain ID
      --home string         directory for config and data (default "/Users/.hazlor")
      --log_format string   The logging format (json|plain) (default "plain")
      --log_level string    The logging level (trace|debug|info|warn|error|fatal|panic) (default "info")
      --trace
```

:::

## Keys management - `hazlord keys`

First of all, you will need an address to store and spend your CRO.

### `keys add <wallet_name>` - Create a new key

You can create a new key with the name `Default` as in the following example:
::: details Example: Create a new address

```bash
$ hazlord keys add Default
- name: Default
  type: local
  address: tcrc1r4erhyx6jk8nsafhlw7263upnw9hja90gdgj5d
  pubkey: '{"@type":"/ethermint.crypto.v1alpha1.ethsecp256k1.PubKey","key":"A3EzNez+oPwDnRTY9OWdVDSjOqikiP7zYncTyxil2SgO"}'
  mnemonic: ""


**Important** write this mnemonic phrase in a safe place.
It is the only way to recover your account if you ever forget your password.

farm surround surround hunt shop glory fringe bag mountain clerk arch ankle announce turtle slide brisk carbon album immense drop example speed grain dutch
```

:::
The key comes with a "mnemonic phrase", which is serialized into a human-readable 24-word mnemonic. User can recover their associated addresses with the mnemonic phrase.

:::danger
It is important that you keep the mnemonic for address secure, as there is **no way** to recover it. You would not be able to recover and access the funds in the wallet if you forget the mnemonic phrase.
:::

### `keys add <key_name> --recover ` - Restore existing key by seed phrase

You can restore an existing key with the mnemonic.

::: details Example: Restore an existing key

```bash
$ hazlord keys add Default_restore --recover
> Enter your bip39 mnemonic
## Enter your 24-word mnemonic here ##
```

:::

### `keys list` - List your keys

Multiple keys can be created when needed. You can list all keys saved under the storage path.

::: details Example: List all of your keys

```bash
$ hazlord keys list
    - name: Default
    type: local
    address: ## Address of "Default" ##
    pubkey: ## Pubkey of "Default" ##
    mnemonic: ""
    threshold: 0
    pubkeys: []
  - name: Default_restore
    type: local
    address: ## Address of "Default_restore" ##
    pubkey: ## Pubkey of "Default_restore" ##
    mnemonic: ""
    threshold: 0
    pubkeys: []
```

:::

### `keys show <key_name>` - Retrieve key information

You can retrieve key information by its name:

::: details Example: Retrieve key information - Account Address and its public key

```bash
$ hazlord keys show mykey --bech acc
- name: mykey
  type: local
  address: tcrc1qsklxwt77qrxur494uvw07zjynu03dq9alwh37
  pubkey: '{"@type":"/ethermint.crypto.v1alpha1.ethsecp256k1.PubKey","key":"A8nbJ3eW9oAb2RNZoS8L71jFMfjk6zVa1UISYgKK9HPm"}'
  mnemonic: ""
```

:::

::: details Example: Retrieve key information - Validator Address and its public key

```bash
$ hazlord keys show Default --bech val
$ hazlord keys show test --bech val
- name: mykey
  type: local
  address: ethvaloper1qsklxwt77qrxur494uvw07zjynu03dq9rdsrlq
  pubkey: '{"@type":"/ethermint.crypto.v1alpha1.ethsecp256k1.PubKey","key":"A8nbJ3eW9oAb2RNZoS8L71jFMfjk6zVa1UISYgKK9HPm"}'
  mnemonic: ""
```

:::

::: details Example: Retrieve key information - Consensus nodes Address and its public key

```bash
$ hazlord keys show Default --bech cons
$ hazlord keys show test --bech cons
- name: mykey
  type: local
  address: ethvalcons1qsklxwt77qrxur494uvw07zjynu03dq9h7rlnp
  pubkey: '{"@type":"/ethermint.crypto.v1alpha1.ethsecp256k1.PubKey","key":"A8nbJ3eW9oAb2RNZoS8L71jFMfjk6zVa1UISYgKK9HPm"}'
  mnemonic: ""
```

:::

### `keys delete <key_name>` - Delete a key

You can delete a key in your storage path.

:::danger
Make sure you have backed up the key mnemonic before removing any of your keys, as there will be no way to recover your key without the mnemonic.
:::

::: details Example: Remove a key

```bash
$ hazlord keys delete Default_restore1
Key reference will be deleted. Continue? [y/N]: y
Key deleted forever (uh oh!)
```

:::

### `keys export <key_name>` - Export private keys

You can export and backup your key by using the `export` subcommand:

::: details Example: Export your keys
Exporting the key _Default_ :

```bash
$ hazlord keys export Default
Enter passphrase to encrypt the exported key: ## Insert passphrase (must be at least 8 characters)##
-----BEGIN TENDERMINT PRIVATE KEY-----
kdf: bcrypt
salt: ## Salt of the key ##
type: secp256k1

## Tendermint private key ##
-----END TENDERMINT PRIVATE KEY-----
```

:::

### The keyring `--keyring-backend` option

Interacting with a node requires a public-private key pair. Keyring is the place holding the keys. The keys can be stored in different locations with specified backend type.

```
$ hazlord keys [subcommands] --keyring-backend [backend type]
```

### `os` backend

The default `os` backend stores the keys in operating system's credential sub-system, which are comfortable to most users, yet without compromising on security.

Here is a list of the corresponding password managers in different operating systems:

- macOS (since Mac OS 8.6): [Keychain](https://support.apple.com/en-gb/guide/keychain-access/welcome/mac)
- Windows: [Credentials Management API](https://docs.microsoft.com/en-us/windows/win32/secauthn/credentials-management)
- GNU/Linux:
  - [libsecret](https://gitlab.gnome.org/GNOME/libsecret)
  - [kwallet](https://api.kde.org/frameworks/kwallet/html/index.html)

### `file` backend

The `file` backend stores the encrypted keys inside the app's configuration directory. A password entry is required everytime a user access it, which may also occur multiple times of repeated password prompts in one single command.

### `test` backend

The `test` backend is a password-less variation of the `file` backend. It stores unencrypted keys inside the app's configuration directory. It should only be used in testing environments and never be used in production.

## Transactions subcommands - `hazlord tx`

### `tx bank send ` - Transfer operation

Transfer operation involves the transfer of tokens between two addresses.

#### **Send Funds** [`tx bank send <from_key_or_address> <to_address> <amount> <network_id>`]

:::details Example: Send 10tcro  from an address to another.

```bash
$ hazlord tx bank send Default tcrc1gjdxrv77zfpq6cywcs8kg6gqyfhl5768ucel6t 10tcro  --chain-id cronostestnet_338-1
  ## Transaction payload##
  {"body":{"messages":[{"@type":"/cosmos.bank.v1beta1.MsgSend","from_address"....}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

### `tx staking` - Staking operations

Staking operations involve the interaction between an address and a validator. It allows you to create a validator and lock/unlocking funds for staking purposes.

#### **Delegate you funds to a validator** [`tx staking delegate <validator-addr> <amount>`]

To bond funds for staking, you can delegate funds to a validator by the `delegate` command

::: details Example: Delegate funds from `mykey` to a validator under the address `ethvaloper...lq`

```bash
$ hazlord tx staking delegate ethvaloper1qsklxwt77qrxur494uvw07zjynu03dq9rdsrlq 100tcro --from mykey --chain-id cronostestnet_338-1
## Transactions payload##
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgDelegate"....}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

#### **Unbond your delegated funds** [`tx staking unbond <validator-addr> <amount>`]

On the other hand, we can create a `Unbond` transaction to unbond the delegated funds

::: details Example: Unbond funds from a validator under the address `ethvaloper...lq`

```bash
$ hazlord tx staking unbond ethvaloper1qsklxwt77qrxur494uvw07zjynu03dq9rdsrlq 100tcro --from mykey --chain-id cronostestnet_338-1
## Transaction payload##
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgUndelegate"...}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

::: tip

- Once your funds were unbonded, It will be locked until the `unbonding_time` has passed.

:::

## Balance & transaction history

### `query bank balances` - Check your transferable balance

You can check your _transferable_ balance with the `balances` command under the bank module.
:::details Example: Check your address balance

```bash
$ hazlord query bank balances tcrc1a303tt49l5uhe87yaneyggly83g7e4uncdxqtl --output json | jq

{
  "balances": [
    {
      "denom": "basetcro",
      "amount": "99999000000000000000000000"
    }
  ],
  "pagination": {
    "next_key": null,
    "total": "0"
  }
}

```

:::

## Advance operations and transactions

### `tx staking create-validator` - Joining the network as a validator

Anyone who wishes to become a validator can submit a `create-validator` transaction by

```bash
$ hazlord tx staking create-validator [flags]
```

::: details Example: Joining the network as a validator

```bash
$ hazlord tx staking create-validator \
--amount="100cro" \
--pubkey='{"@type":...,"key":...}' \
--moniker="The_new_node" \
--chain-id="cronostestnet_338-1" \
--commission-rate="0.10" \
--commission-max-rate="0.20" \
--commission-max-change-rate="0.01" \
--min-self-delegation="1" \
--from=node1
## Transactions payload##
{"body":{"messages":[{"@type":"/cosmos.staking.v1beta1.MsgCreateValidator"...}
confirm transaction before signing and broadcasting [y/N]: y
```

:::

(TODO: details of each flag )

### `tx slashing unjail` - Unjail a validator

Validator could be punished and jailed due to network misbehaviour, for example if we check the validator set:

```bash
$ hazlord query staking validators -o json | jq
................................
    "operator_address": "ethvaloper1zwm45n5r3u3xcpsd00d3arwzhz7250rtsadv65",
    "consensus_pubkey": {
        "@type": "/cosmos.crypto.ed25519.PubKey",
        "key": "fD6cWVYv5rsNbXDw3hVIbB3nd9x57HsTyeMgwmH472U="
    },
    "jailed": false,
    "status": "BOND_STATUS_BONDED",
................................
```

After the jailing period has passed, one can broadcast a `unjail` transaction to unjail the validator and resume its normal operations by

```bash
$ hazlord tx slashing unjail --from node1 --chain-id cronostestnet_338-1
  {"body":{"messages":[{"@type":"/cosmos.slashing.v1beta1.MsgUnjail"...}]}
  confirm transaction before signing and broadcasting [y/N]: y
```
