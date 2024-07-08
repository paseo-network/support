# Parachain Onboarding to Paseo Testnet Wiki

## Introduction

Welcome to the Paseo Testnet onboarding guide for parachains. This document provides a comprehensive guide for projects looking to join the Paseo Testnet as a parachain. Here you'll find step-by-step instructions, required configurations, and best practices to ensure your integration is successful.

## Table of Contents

1. **Prerequisites**
2. **Parachain Registration Process**
3. **Setting Up Your Parachain**
4. **Monitoring and Maintenance**
5. **FAQ**
6. **Support and Resources**

## 1. Prerequisites

Before initiating the onboarding process, ensure you meet the following requirements:

- **Technical Requirements:** Have a fully operational blockchain capable of connecting as a parachain. This includes having a collator setup ready.
- **Resource Allocation:** Ensure you have the computational and network resources to run a stable collator node.
- **Paseo Tokens:** Acquire sufficient PAS tokens for staking if required for becoming a parachain on the Paseo Testnet.
- **Paseo Tokens:** Acquire sufficient PAS tokens to register your chain's state and code. The faucet is available for everybody to use as means of getting PAS https://faucet.polkadot.io.

If having troubles, please open an issue at our support repo [Paseo Testnet Support Repository](https://github.com/paseo-network/support/issues)

## 2. Parachain Registration Process

To register your blockchain as a parachain on the Paseo Testnet:
### 2.1 Application Submission
- Submit an application by opening an issue in the [Paseo Testnet Support Repository](https://github.com/paseo-network/support/issues). Include technical details of your blockchain, team information, and your objectives for joining the testnet.
### 2.2 Review and Approval
- The Paseo team will review your submission. Approval is based on the readiness of your technology, the potential impact on the Paseo ecosystem, and the availability of parachain slots.
### 2.3 Slot Allocation
- While coretime is not yet live, feel free to request a slot directly by contacting the Paseo team. Detailed procedures for requesting a slot are provided upon contact. For more details, please refer to [PAS#9](https://github.com/paseo-network/paseo-action-submission/blob/main/pas/PAS-9-Onboard-paras-slots.md).

## 3. Setting Up Your Parachain
- Configure your blockchain’s collator nodes so that they use the [Paseo Spec](https://github.com/paseo-network/runtimes/blob/main/chain-specs/paseo.raw.json). It already includes the relevant bootnodes.
Your launch script might look similar to:
```bash
./target/release/parachain-template-node \
--alice \
--collator \
--chain my_parachain_spec.json \
-- \
--chain paseo.raw.json \
```

## 4. Monitoring and Maintenance
### 4.1 Monitoring Tools
- Implement monitoring tools to keep track of the parachain’s performance and stability. For instance, most of the nodes connected to Paseo network are visible at its [telemtry endpoint](https://telemetry.polkadot.io/#list/0x77afd6190f1554ad45fd0d31aee62aacc33c6db0ea801129acb813f913e0764f).

### 4.2 Updating and Upgrading
- Stay tuned for Team updates regarding upgrades.
Any relevant upgrades happening on Paseo will be posted on [Paseo Announcements channel](https://matrix.to/#/#paseo-announcements:matrix.org).

## 5. FAQ
- WIP

## 6. Support and Resources
- **Technical Support:** 

A vibrant community of developers can be reached out at https://substrate.stackexchange.com/

- **Documentation:** 

Some interesting resources for launching collators can be found at this devops guide chapter: [Deploying  a Parachain](https://paritytech.github.io/devops-guide/guides/parachain_deployment.html).

- **Community:** 

Join Paseo's public room at https://matrix.to/#/#paseo-testnet-support:parity.io or at Polkadot's discord server.
