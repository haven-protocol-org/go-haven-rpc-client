Go Haven RPC Client
====================

<p align="center">
<img src="https://github.com/monero-ecosystem/go-monero-rpc-client/raw/master/media/img/monero_gopher.png" alt="Monero Gopher" width="200" />
</p>

A client implementation for the Haven wallet and daemon RPC written in go.
This repo is a fork of https://github.com/monero-ecosystem/go-monero-rpc-client and modified for Haven.

## Wallet RPC Client

[![GoDoc](https://godoc.org/github.com/monero-ecosystem/go-monero-rpc-client/wallet?status.svg)](https://godoc.org/github.com/monero-ecosystem/go-monero-rpc-client/wallet)

### Haven RPC Version
The ```go-haven-rpc-client/wallet``` package is the RPC client of the [Haven Wallet RPC](https://docs.havenprotocol.org/rpc/1.2.9q/RPC_API.html) for the avaible methods and 
the rest of the methods can be found in for version `v1.3` of the [Monero Wallet RPC](https://www.getmonero.org/resources/developer-guides/wallet-rpc.html).

### Installation

```sh
go get -u https://github.com/haven-protocol-org/go-haven-rpc-client
```

#### Spawn the haven-wallet-rpc daemon (without rpc login):

```sh
./haven-wallet-rpc --wallet-file /home/$user/stagenetwallet/stagenetwallet --stagenet --rpc-bind-port 6061 --password 'mystagenetwalletpassword' --disable-rpc-login
```

#### Go code:

```Go
package main

import (
  "encoding/json"
  "fmt"
  "log"

  "github.com/haven-protocol-org/go-haven-rpc-client/wallet"
)

func checkerr(err error) {
  if err != nil {
    log.Panic(err)
  }
}

func main() {
  // Start a wallet client instance
  client := wallet.New(wallet.Config{
    Address: "http://127.0.0.1:6061/json_rpc",
  })

  // check wallet balance
  resp, err := client.GetBalance(&wallet.RequestGetBalance{AccountIndex: 0, AssetType: "XHV"})
  checkerr(err)
  res, _ := json.MarshalIndent(resp, "", "\t")
  fmt.Print(string(res))

  // get incoming transfers
  resp1, err := client.GetTransfers(&wallet.RequestGetTransfers{
    AccountIndex: 0,
    In:           true,
  })
  checkerr(err)
  for _, in := range resp1.In {
    res, _ := json.MarshalIndent(in, "", "\t")
    fmt.Print(string(res))
  }
}
```

### Spawn the monero-wallet-rpc daemon (with rpc login):

```sh
./haven-wallet-rpc --wallet-file /home/$user/stagenetwallet/stagenetwallet --stagenet --rpc-bind-port 6061 --password 'mystagenetwalletpassword' --rpc-login test:testpass
```

#### Go code:

```Go
package main

import (
  "encoding/json"
  "fmt"
  "log"

  "github.com/haven-protocol-org/go-haven-rpc-client/wallet"
)

func checkerr(err error) {
  if err != nil {
    log.Panic(err)
  }
}

func main() {
  t := httpdigest.New("test", "testpass")

  // Start a wallet client instance
  client := wallet.New(wallet.Config{
    Address: "http://127.0.0.1:6061/json_rpc",
    Transport: t,
  })

  // check wallet balance
  resp, err := client.GetBalance(&wallet.RequestGetBalance{AccountIndex: 0, AssetType: "XHV"})
  checkerr(err)
  res, _ := json.MarshalIndent(resp, "", "\t")
  fmt.Print(string(res))

  // get incoming transfers
  resp1, err := client.GetTransfers(&wallet.RequestGetTransfers{
    AccountIndex: 0,
    In:           true,
  })
  checkerr(err)
  for _, in := range resp1.In {
    res, _ := json.MarshalIndent(in, "", "\t")
    fmt.Print(string(res))
  }
}
```

# Daemon RPC Client

As of now, only the wallet RPC has been implemented. The daemon RPC will follow very soon.

# Contribution
* You can fork this, extend it and contribute back.
* You can contribute with pull requests.

# LICENSE
MIT License