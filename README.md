# GSN v3 integration workshop

## (This branch adds a custom paymaster to our GSN-enabled sample)

This sample dapp emits an event with the last account that clicked on the "capture the flag" button. We will integrate
this dapp to work gaslessly with GSN v3. This will allow an externally owned account without ETH to capture the flag by
signing a meta transaction.

### To run the sample

1. first clone and `yarn` to install dependencies
2. run `yarn gsn-with-ganache` to start a node, and also deploy GSN contracts and start a relayer service.
3. open a new terminal, run `yarn add truffle`, then `npx truffle init` to create `truffle-config.js`
4. Make sure you have Metamask installed, and pointing to "localhost"
5. In a different window, run `yarn start`, to deploy the contract, and start the UI
6. Start a browser pointing to "http://localhost:3000"
7. Click the "Capture the Flag" button. Notice that you don't need eth in your account: You only sign the transaction.

You can see the integrations as GitHub pull requests:

1. [Basic: Minimum viable GSN integration](https://github.com/opengsn/workshop/pull/1/files)
2. (this branch) [Advanced: Write your own custom Paymaster](https://github.com/opengsn/workshop/pull/2/files_)

Note: on testnet we maintain a public service "pay for everything" paymaster so writing your own is not strictly
required. On mainnet, you need a custom paymaster, such as a token paymaster that allow users to pay for gas in tokens,
and exchanging them for ETH ETH on Uniswap. Dapps will want to develop their own custom paymaster in order, for example
to subsidize gas fees for new users during the onboarding process.

### To deploy demo smart contract

Please ensure that you have enabled the network config in `truffle-config.js` before `yarn start`

```js
...

 networks: {
    // Useful for testing. The `development` name is special - truffle uses it by default
    // if it's defined here and no other network is specified at the command line.
    // You should run a client (like ganache, geth, or parity) in a separate terminal
    // tab if you use this network and you must also set the `host`, `port` and `network_id`
    // options below to some value.
    //
    development: {
     host: "127.0.0.1",     // Localhost (default: none)
     port: 8545,            // Standard Ethereum port (default: none)
     network_id: "*",       // Any network (default: none)
    },
...
```

### To fix compile error on the smart contract

Just remove `override` from `CaptureTheFlag.sol`

```bash
TypeError: Public state variable has override specified but does not override anything.
  --> project:/contracts/CaptureTheFlag.sol:18:19:
   |
18 |     string public override versionRecipient = "3.0.0";
   |                   ^^^^^^^^

Compilation failed. See above.
```

### To interact with gsn cli

```bash
npx gsn
```

### To add your address in whitelist paymaster

At `2_deploy_contracts.js`, try to add your metamask wallet address to `whitelistSender()`

After added, the smart contract will allow you to interact with the web demo.

```js
...
// This is the first ganache address, when started with "ganache-cli -d"
// you can add your metamask address here.
await paymaster.whitelistSender("0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1");
// await paymaster.whitelistSender('<your address>') ***

// you can also add addresses by running `truffle console` and then running:
// const pm = await WhitelistPaymaster.deployed()
// pm.whitelistSender('0x90F8bf6A479f320ead074411a4B0e7944Ea8c9C1')
...
```

### Further reading

GSNv3 integration tutorial: <https://docs.opengsn.org/relay-server/tutorial.html>

Documentation explaining how everything works: <https://docs.opengsn.org/>
