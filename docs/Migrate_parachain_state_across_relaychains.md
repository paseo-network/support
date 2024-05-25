# Migrate parachain state AND history across relaychains
## Pre-conditions

* The parachains has a way to disable relaychain block number checks. KILT has developed a solution in this pallet. Other alternative solutions might exist.

## Steps
1- Set the relaychain block number checks to false

2- Stop all collators

3 - Wait for the latest head to be finalised in the relaychain, make sure it does not change anymore for 1 minute or so

4 - Take note of the parachain head as stored on the relaychain under the given para ID

5 - Copy-paste the old chainspec into a new chainspec, changing the relative fields, e.g., paraId, protocolId, etc.

6 - If a new id is used for the new chainspec (recommended), then access the state DB of each collator and fullnode, optionally make a backup of each if needed, and rename the parachain DB, which is typically under para/chains/<chainspec_id> to para/chains/<new_chainspec_id>. The state under relay/chains/<relay_chainspec_id> can most likely be deleted altoghether if the parachain is switching relaychain.

7 - Start all nodes that are NOT collators with the new chainspec, potentially using --sync warp for the relaychain component to make syncing faster

8 - If no warnings arise, once the non-collating nodes are up to speed, start and sync also the collators in the same way with the new chainspec.

9 - Onboard the parachain on the new relaychain using the new paraID, using the head retrieved at step 4 and the same WASM that the parachain was using.

10 - Verify collators can produce blocks

11 - On the parachain, set raw storage for the parachainInfo pallet to the new para ID, if different than before

12 - Re-enable relaychain block number checks, using the reverse of the mechanism used in step 1.

13 - Enjoy!

## Contributor
Thanks to Antonio for the contribution made on https://hackmd.io/@4Zchucf_QeG8XyIumgx7Dg/rJ2pD_7XA
