# Kadena.ex
![Build Badge](https://img.shields.io/github/workflow/status/kommitters/kadena.ex/Kadena%20CI/main?style=for-the-badge)
[![Coverage Status](https://img.shields.io/coveralls/github/kommitters/kadena.ex?style=for-the-badge)](https://coveralls.io/github/kommitters/kadena.ex)
[![Version Badge](https://img.shields.io/hexpm/v/kadena?style=for-the-badge)](https://hexdocs.pm/kadena)
![Downloads Badge](https://img.shields.io/hexpm/dt/kadena?style=for-the-badge)
[![License badge](https://img.shields.io/hexpm/l/kadena?style=for-the-badge)](https://github.com/kommitters/kadena.ex/blob/main/LICENSE)

**Kadena.ex** is an open source library for Elixir that allows developers to interact with the Kadena Chainweb.

## What can you do with Kadena.ex?

- Construct commands for transactions.
- Implement cryptography required by the network.
- Interacting with public network endpoints:
  - listen, local, poll, send, spv, cut.
- Send, test and update smart contracts on the network.

## Installation

Add `kadena` to your list of dependencies in `mix.exs`:

```elixir
def deps do
  [
    {:kadena, "~> 0.8.0"}
  ]
end
```

## Roadmap

The latest updated branch to target a PR is `v0.8`

You can see a big picture of the roadmap here: [**ROADMAP**][roadmap]

### What we're working on now 🎉

- [Kadena Chainweb](https://github.com/kommitters/kadena.ex/issues/57)

### Done - What we've already developed! 🚀

<details>
<summary>Click to expand!</summary>

- [Base types](https://github.com/kommitters/kadena.ex/issues/11)
- [Keypair types](https://github.com/kommitters/kadena.ex/issues/12)
- [PactValue types](https://github.com/kommitters/kadena.ex/issues/15)
- [SignCommand types](https://github.com/kommitters/kadena.ex/issues/16)
- [ContPayload types](https://github.com/kommitters/kadena.ex/issues/28)
- [Cap types](https://github.com/kommitters/kadena.ex/issues/30)
- [ExecPayload types](https://github.com/kommitters/kadena.ex/issues/32)
- [PactPayload types](https://github.com/kommitters/kadena.ex/issues/34)
- [MetaData and Signer types](https://github.com/kommitters/kadena.ex/issues/35)
- [CommandPayload types](https://github.com/kommitters/kadena.ex/issues/36)
- [PactExec types](https://github.com/kommitters/kadena.ex/issues/40)
- [PactEvents types](https://github.com/kommitters/kadena.ex/issues/41)
- [CommandResult types](https://github.com/kommitters/kadena.ex/issues/43)
- [PactCommand types](https://github.com/kommitters/kadena.ex/issues/13)
- [PactAPI types](https://github.com/kommitters/kadena.ex/issues/17)
- [Wallet types](https://github.com/kommitters/kadena.ex/issues/18)
- [Kadena Crypto](https://github.com/kommitters/kadena.ex/issues/51)
- [Kadena Pact](https://github.com/kommitters/kadena.ex/issues/55)
- [Kadena Pact Commands Builder](https://github.com/kommitters/kadena.ex/issues/131)

</details>

---

## Building Commands

### Command

It allows construction of PACT command payloads, in a composable and semantic manner, which will be part of the body of a request to the pact api endpoints, the commands which allows to build are [Execution](#execution-command) command and `Continuation` command

### [Execution Command](#execution-command)

An execution command is the first contract state that contains the actor keys and the code that will be run once the contract's requirements have been met.
To create an execution command is needed:

- [Network Id](#network-id)
- Codigo (PACT)
- Nonce
- [Enviroment Data](#env-data)
- [Meta Data](#meta-data)
- [Key Pairs](#key-pair)
- [Signers](#signers-list)

### [Network ID](#network-id)

To create a network ID:

```elixir
Kadena.Types.NetworkID.new(:testnet04)
```

### [Env Data](#env-data)

To create an Enviroment data:

```elixir
data= %{
  accounts_admin_keyset: [
    "ba54b224d1924dd98403f5c751abdd10de6cd81b0121800bf7bdbdcfaec7388d"    
  ]
}

Kadena.Types.EnvData.new(data)
```

### [Meta Data](#meta-data)
To create a Meta data:
```elixir
[
  creation_time: 0,
  ttl: 0,
  gas_limit: 2500,
  gas_price: 1.0e-2,
  sender: "account_name",
  chain_id: "0"
]
|>Kadena.Types.MetaData.new()
```

### [Key Pair](#key-pair)
Here are two ways to create a keypair:
```elixir
#return new public and the secret keys
Kadena.Cryptography.KeyPair.generate()

#with a secret key already in existence

secret_key = "secret_key_value"
{:ok, %KeyPair{pub_key: pub_key}} = CryptographyKeyPair.from_secret_key(secret_key)
```

The KeyPair can then be created either with or without Capabilities

```elixir
#without Capabilities
[
pub_key: "pub_key_value",
secret_key: "secret_key_value"
]
|> KeyPair.new()
#with Capabilities
clist =
      CapsList.new([
        [name: "gas", args: ["COIN.gas", 0.02]],
        [name: "transfer", args: ["COIN.transfer", "key_1", 50, "key_2"]]
      ])
[
pub_key: "pub_key_value",
secret_key: "secret_key_value",
clist: clist
]
|> KeyPair.new()
```

### [Signers List](#signers-list)
There are many ways to create a list of signatures but we recommend 
```elixir
signer1 = [
        pub_key: "pub_key_1",
      ]

signer2 = [
        pub_key: "pub_key_2",
      ]

Kadena.Types.SignersList.new([signer1, signer2])
```
---

## Development

- Install any Elixir version above 1.13.
- Compile dependencies: `mix deps.get`
- Run tests: `mix test`.

## Want to jump in?

Check out our [Good first issues][good-first-issues], this is a great place to start contributing if you're new to the project!

We welcome contributions from anyone! Check out our [contributing guide][contributing] for more information.

## Changelog

Features and bug fixes are listed in the [CHANGELOG][changelog] file.

## Code of conduct

We welcome everyone to contribute. Make sure you have read the [CODE_OF_CONDUCT][coc] before.

## Contributing

For information on how to contribute, please refer to our [CONTRIBUTING][contributing] guide.

## License

This library is licensed under an MIT license. See [LICENSE][license] for details.

## Acknowledgements

Made with 💙 by [kommitters Open Source](https://kommit.co)

[license]: https://github.com/kommitters/kadena.ex/blob/main/LICENSE
[coc]: https://github.com/kommitters/kadena.ex/blob/main/CODE_OF_CONDUCT.md
[changelog]: https://github.com/kommitters/kadena.ex/blob/main/CHANGELOG.md
[contributing]: https://github.com/kommitters/kadena.ex/blob/main/CONTRIBUTING.md
[roadmap]: https://github.com/orgs/kommitters/projects/5/views/3
[good-first-issues]: https://github.com/kommitters/kadena.ex/labels/%F0%9F%91%8B%20Good%20first%20issue
