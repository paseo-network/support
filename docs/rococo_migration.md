# Migrate a parachain from Rococo to Paseo

The migration process that is done in this guide is a hard spoon. Meaning that
the current state and code from the running network will be taken and used as
genesis for the parachain being registered in Paseo.

If this process is not of your interest, please continue with a usual parachain
onboarding.

---

The very first step before initiating the migration would be stopping all the collators.
Once all your machines are not executing your parachain binaries, then it is safe to proceed.

The migration process is rather simple. One just needs to open an issue here:

- [Migration Issue](https://github.com/paseo-network/support/issues/new/choose)

In the issue tempalate there is space to fill the following information:

### **ParaId**

The Id of the parachain running in Rococo we want to migrate. This Id will be used to
retrieve the following data:
- `paras.heads(ParaId)` 
- `paras.currentCodeHash(ParaId)` which at the same time will be used to retrieve the actual code
for this parachain.
- `registrar.paras(ParaId)` from this struct the current manager account will be extrancted.

And will be the Id used to register the new parachain in Paseo. Avoiding innecessary changes in the parachain codebase.

### **Manager Account**

Ideally the same account than in Rococo will be used as a way of streamlining the process.

### **Proof of Ownership**

As a way of proving that the creator of the issue has access to the manager account. This proof could be requested if the reviewers can't identify the user making the request.
Note this can be done in various way, for instance, using polkadot.js/apps - https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frococo-rpc.polkadot.io#/signing

Another way to generate such proof: use the `sign` subcommand that ships with substrate. A bianry that contains such subcommand is for isntance `staging-node-cli` in [`polkadot-sdk/substrate/bin/node`](https://github.com/paritytech/polkadot-sdk/tree/master/substrate/bin/node).
Which can be built like this:
```bash
$ cargo build --release -p staging-node-cli
```
> Bear in mind that the resulting binary is called `substrate-node`

Once the binary with this subcommand is ready the message to sing is `PASEO`. Which can be achieved running:
```bash
./substrate-node sign --message PASEO --suri <secret>
```
> Note that there are various options of providing the secret, pelase check the output of `sign --help`.

The output of the execution shoul be something similar to 
`0xbaa49812c9ddd70bb8514ba3841a25a8117c2c33cedde5229e4e1f058ce24f57281c45afb10c1dc094d2c3438cdc61884a9a814cd34358a41b017e3755734b8a` 

_(In my tests I have been observing the output including a character `%` at the end of the signature, which is not part of it.)_

---

Once done you should see that your parachain is being registered and ready to start onboarding.

Be mindful of your "old" parachain's database, you might want to back it up or you might choose to simply purge it. But this could be a good moment to address it.

Now everything should be ready to run the collators again pointing now to the right chain specs.

Paseo's raw spec can be found at:
- [paseo-network/runtimes/chain-specs](https://github.com/paseo-network/runtimes/tree/main/chain-specs)
- [polkadot-sdk/polkadot/node/service/chain-specs](https://github.com/paritytech/polkadot-sdk/tree/master/polkadot/node/service/chain-specs)