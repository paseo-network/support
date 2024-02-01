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

- [Migration Issue]()

In the issue tempalate there is space to fill the following information:

**ParaId**
The Id of the parachain running in Rococo we want to migrate. This Id will be used to
retrieve the following data:
- `paras.heads(ParaId)` 
- `paras.currentCodeHash(ParaId)` which at the same time will be used to retrieve the actual code
for this parachain.
- `registrar.paras(ParaId)` from this struct the current manager account will be extrancted.

**Manager Account**
Ideally the same account than in Rococo will be used as a way of streamlining the process.

**Proof of Ownership**
As a way