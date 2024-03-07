# Setting Up a Node/Validator for Paseo Testnet

Welcome to the Paseo testnet! This guide will walk you through the process of setting up a node and becoming a validator on the Paseo testnet network using the Polkadot binary, which is also used for Polkadot and Kusama validators.

## Prerequisites

Before you begin, make sure you have the following:

- A server with a recommended minimum of 8GB RAM, a quad-core CPU, and at least 200GB of free disk space.
- The latest version of the Polkadot binary installed on your server. You can download it from [the official Polkadot repository](https://github.com/paritytech/polkadot-sdk/releases).
- Basic knowledge of terminal/command-line interface.

## 1. Download the Chain Specification

First, you need to download the Paseo chain specification file:

```shell
wget https://github.com/paseo-network/runtimes/blob/main/chain-specs/paseo.raw.json
```

## 2. Initialize Your Node
With the chain specification file downloaded, you will initialize your node. This step prepares your node to run by setting up the necessary directories and configuration files.

```shell
polkadot --chain paseo.raw.json --base-path /path/to/your/node/data/directory
```
Replace /path/to/your/node/data/directory with the actual path where you want your node's data to be stored.

## 3. Running Your Node
To start your node and connect it to the Paseo testnet, run the following command:

```shell
polkadot --chain paseo.raw.json --name 'Your Node Name' --pruning archive
```
Replace 'Your Node Name' with a unique name for your node. The --pruning archive flag is recommended for validators to ensure you retain all historical data.

## 4. Becoming a Validator
Setting up a validator is trivial once your node is synchronized with the network. However, you need to ensure you have a secure setup, especially for key management and exposure to the internet. Validators participate in consensus and, therefore, require a higher level of security.

Please follow the Polkadot Validator Guide for detailed instructions on secure validator setup: [Polkadot Validator Setup](https://wiki.polkadot.network/docs/maintain-guides-how-to-validate-polkadot)

While the guide is for Polkadot, the principles and steps apply to Paseo as well. Just make sure to use the paseo.raw.json chain spec when prompted.

## 5. Monitoring and Maintenance
Once your validator is set up, it's crucial to monitor its performance and keep it updated. Regularly check for updates to the Polkadot binary and apply them as needed. Also, monitor your node's logs for any errors or warnings.

# Conclusion
Congratulations! You have set up a node and are on your way to becoming a validator on the Paseo testnet. By participating as a validator, you're contributing to the network's security and operation.

For support and further discussions, join our community channels on Element.
