[geth readme](README.original.md)

# Builder API

Builder API implementing [builder spec](https://github.com/ethereum/builder-specs), making geth into a standalone block builder. 

Run on your favorite network, including Kiln and local devnet.

Requires forkchoice update to be sent for block building, on public testnets run beacon node modified to send forkchoice update on every slot [example modified beacon client (lighthouse)](https://github.com/flashbots/lighthouse)

Test with [mev-boost](https://github.com/flashbots/mev-boost) and [mev-boost test cli](https://github.com/flashbots/mev-boost/tree/main/cmd/test-cli).

Provides summary page at the listening address' root (http://localhost:28545 by default).

## How it works

Builder API has two hooks into geth:
* On forkchoice update, changing the payload attributes feeRecipient to the one registered for next slot's validator
* On new sealed block, consuming the block as the next slot's proposed payload

## Limitations

* Blocks are only built on forkchoice update call from beacon node
* Only works post-Bellatrix, fork version is static
* Does not accept external blocks
* Does not have payload cache, only the latest block is available

## Usage

Configure geth for your network, it will become the block builder.

Builder API options:
```
$ geth --help
BUILDER API OPTIONS:
  --builder.validator_checks               Enable the validator checks
  --builder.secret_key value               Builder API key used for signing headers (default: "0x2fc12ae741f29701f8e30f5de6350766c020cb80768a0ff01e6838ffd2431e11") [$BUILDER_SECRET_KEY]
  --builder.listen_addr value              Listening address for builder endpoint (default: ":28545") [$BUILDER_LISTEN_ADDR]
  --builder.genesis_fork_version value     Gensis fork version (default: "0x02000000") [$BUILDER_GENESIS_FORK_VERSION]
  --builder.bellatrix_fork_version value   Bellatrix fork version (default: "0x02000000") [$BUILDER_BELLATRIX_FORK_VERSION]
  --builder.genesis_validators_root value  Genesis validators root of the network (static). For kiln use 0x99b09fcd43e5905236c370f184056bec6e6638cfc31a323b304fc4aa789cb4ad (default: "0x0000000000000000000000000000000000000000000000000000000000000000") [$BUILDER_GENESIS_VALIDATORS_ROOT]
  --builder.beacon_endpoint value          Beacon endpoint to connect to for beacon chain data (default: "http://127.0.0.1:5052") [$BUILDER_BEACON_ENDPOINT]
```
