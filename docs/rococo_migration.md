# Migrate a parachain from Rococo to Paseo

This document presents two options for teams to migrate their running parachains to Paseo testnet:

1. [**Re-genesis**](#1-re-genesis): The state at a certain block# of your chain will be the genesis of the chain onboarded to Paseo. Block will start producing from #0.
2. [**Migrate state and history**](#2-migrate-state-and-history): The migration will maintain the state as well the block history of the chain being migrated. It will stop producing blocks on one relay chain to produce them on Paseo testnet.


_If you are not looking for migrating a parachain from any other relay chain, please continue with a usual parachain onboarding as described [here](https://github.com/paseo-network/paseo-action-submission/blob/main/pas/PAS-9-Onboard-paras-slots.md)._

---

## 1. Re-genesis

### Pre-requisites

Let's choose a block# to export the state of the running parachain and use that state for the new genesis in Paseo.
The bock# to choose could be any, ideally some block close to the migration date, or one at some relevant event which state you want included in the new genesis.
Now export the chain state with the following command:

`$ ./parachain-binary export-state [--chain <your-running-spec> --database <rocksdb/paritydb/auto>] <BLOCK# or BLOCK-HASH> > parachain-spec-for-paseo.json`

> This spec might include a value for the key `0x45323df7cc47150b3930e2666b0aa313a2bca190d36bd834cc73a38fc213ecbd`. Please, feel free to remove this one from the spec. It corresponds to the last relay block the parachain has seen. In case Paseo block is < than the value for this key this would lead to a runtime panic. Blocking your parachain from producing blocks.

Once that spec is ready it can be used to export the genesis wasm and state that will be used to register your parachain in Paseo.

`$ ./parachain-binary export-genesis-state --chain <parachain-spec-for-paseo.json > parachain-genesis-head-for-paseo`

`$ ./parachain-binary export-genesis-wasm --chain <parachain-spec-for-paseo.json > parachain-genesis-wasm-for-paseo>`


### Migration: Onboarding to Paseo

The migration process is rather simple. One just needs to open an issue here:

- [Paseo Parachain Onboarding Issue](https://github.com/paseo-network/support/issues/new/choose)

In the issue tempalate there is space to fill the following information:

### **ParaId**

The Id of the parachain running in Rococo we want to migrate. Avoiding this way runtime modifications.


### **Parachain Manager Account**

Ideally the same account than in Rococo will be used as a way of streamlining the process.
Otherwise any account you control in Paseo is fine.

### **Genesis State**

Upload the genesis state generated in the [pre-requisites](#pre-requisites) section.

> __Notice that if you plan to attach these files into a GitHub issue or comment, you might need to append a file formart to the file names, as per GitHub restrictions._

### **Validation Code**

Upload the genesis wasm generated in the [pre-requisites](#pre-requisites) section.

> __Notice that if you plan to attach these files into a GitHub issue or comment, you might need to append a file formart to the file names, as per GitHub restrictions._

---

Once this is done, Paseo maintainers will start your onboarding.

Be mindful of your "old" parachain's database, you might want to back it up or you might choose to simply purge it. But this could be a good moment to address it.

Now everything should be ready to run your collators pointing them to the [correct Paseo spec](https://github.com/paseo-network/runtimes/blob/main/chain-specs/paseo.raw.json).


---
## 2. Migrate state and history

This section has been contributed by [@ntn-x2](https://github.com/ntn-x2)!

### Pre-requisites

The parachains has a way to disable relaychain block number checks. KILT has developed a solution in [this pallet](https://github.com/KILTprotocol/kilt-node/tree/f42e7aad9ecec992f196c41e7198b3022b60702b/pallets/pallet-configuration). Other alternative solutions might exist.

### Migration

1. Set the relaychain block number checks to false
2. Stop all collators
3. Wait for the latest head to be finalised in the relaychain, make sure it does not change anymore for 1 minute or so
4. Take note of the parachain head as stored on the relaychain under the given `paraId`
5. Copy-paste the old chainspec into a new chainspec, changing the relative fields, e.g., `paraId`, `protocolId`, etc.
6. If a new `id` is used for the new chainspec (recommended), then access the state DB of each collator and fullnode, optionally make a backup of each if needed, and rename the parachain DB, which is typically `under para/chains/<chainspec_id>` to `para/chains/<new_chainspec_id>`. The state under `relay/chains/<relay_chainspec_id>` can most likely be deleted altoghether if the parachain is switching relaychain.
7. Start all nodes that are NOT collators with the new chainspec, potentially using `--sync warp` for the relaychain component to make syncing faster
8. If no warnings arise, once the non-collating nodes are up to speed, start and sync also the collators in the same way with the new chainspec.
9. Onboard the parachain on the new relaychain using the new `paraID`, using the head retrieved at step 4 and the same WASM that the parachain was using. For this check information on [PAS#9](https://github.com/paseo-network/paseo-action-submission/blob/main/pas/PAS-9-Onboard-paras-slots.md)
10. Verify collators can produce blocks
11. On the parachain, set raw storage for the `parachainInfo` pallet to the new para ID, if different than before
12. Re-enable relaychain block number checks, using the reverse of the mechanism used in step 1.
13. Enjoy!

---

### Resouces

Paseo's raw spec can be found at:
- [paseo-network/runtimes/chain-specs](https://github.com/paseo-network/runtimes/tree/main/chain-specs)
- [polkadot-sdk/polkadot/node/service/chain-specs](https://github.com/paritytech/polkadot-sdk/tree/master/polkadot/node/service/chain-specs)