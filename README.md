# bloc

[![BlockApps logo](http://blockapps.net/img/logo_cropped.png)](http://blockapps.net)

[![Join the chat at https://gitter.im/blockapps/bloc](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/blockapps/bloc?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Build Status](https://travis-ci.org/blockapps/bloc.svg)](https://travis-ci.org/blockapps/bloc)

`bloc` is a small command line tool that helps you build blockchain applications on the Ethereum network with the [blockapps api](https://blockapps.net). Bloc makes it effortless to:
* Compile and deploy smart contracts to the blockchain
* Automatically wire those contracts to the front-end, so you can bring the blockchain to the world!

##Installation
Installation is currently done by cloning:

```
git clone https://github.com/blockapps/bloc.git
```

Enter the bloc directory:

```
cd bloc
```

Install bloc as a global package:

```
npm install -g
```

##Generate a new blockchain app

You can use "bloc init" to create a sample app.

```
bloc init
```

![bloc init](https://raw.githubusercontent.com/blockapps/bloc/readme-images/readme_img/bloc_init.png)

bloc init builds a base structure for your blockchain app as well as sets some default parameters values for creating transactions. These can be edited in the config.yaml file in your app directory.

The config.yaml file also holds the app's "apiURL".  This can be configured to point to an isolated test network, or the real Ethereum network.  You can change this link, which will allow you to build and test in a sandboxed environment, and later re-deploy on the real Ethereum blockchain.

You will find the following files in your newly created app directory:

```
/app
  /contracts
    Array.sol
    Payout.sol
    SimpleDataFeed.sol
    SimpleMultiSig.sol
    SimpleStorage.sol
  /css
  /html
  /js
  /lib
  /meta
  /routes
  /views
  app.js
  config.yaml
  gulpfile.js
  package.json
```

An Ethereum app consists of three parts:

-The "contracts" directory holds Ethereum blockchain code, written in the Solidity language, which you can learn about here- https://ethereum.github.io/solidity/docs/home/.  This is the code that will run on the blockchain.  Samples contracts have been provided to get you started.

-The "html", "js", and "css" directories are intended to hold a frontend for your app. The "views" directory contains reusable templates written with [handlebars](http://handlebarsjs.com/) that can be viewed from `bloc`'s embedded webserver.



-Finally, we provide a REST API that will allow you to "glue" your frontend to the code you run in the blockchain.  This API is described at https://strato-dev.blockapps.net/help.



##Creating a Sample Account

Now in your app directory run to download dependencies the app needs

```
npm install
```
Once this is finished run

```
bloc genkey
```

![bloc genkey](https://raw.githubusercontent.com/blockapps/bloc/readme-images/readme_img/bloc_genkey.png)

This generates a new private key and fill it with test-ether (note- free sample Ether is only available on the test network, of course).  You can view the address information in the newly created key.json file.  Also, beware that this file contains your private key, so if you intend to use this address on the live network, make sure you keep this file hidden.

The new account has also been created on the blockchain, and you can view account information by using our REST API directly in a browser by visiting https://strato-dev.blockapps.net/eth/v1.0/account?address= &lt; fill in your address here &gt;

![balance before](https://cloud.githubusercontent.com/assets/5578200/10926491/c5b0bd02-824c-11e5-98d7-3a9e8275a11e.png)


##Uploading Contracts

Getting a contract live on the blockchain is a two step process

1. Compile the contract
2. Upload the contract

To compile your smart contracts

```
bloc compile -s
```

![bloc compile](https://raw.githubusercontent.com/blockapps/bloc/readme-images/readme_img/bloc_compile.png)

If there are any bugs in your contract code, this is where you will be allowed to fix them.

Upload a contract and scaffold (`-s`) your dApp

```
bloc upload <ContractName> -s
```

![bloc upload](https://raw.githubusercontent.com/blockapps/bloc/readme-images/readme_img/bloc_upload.png)

Note that during these steps, javascript scaffolding code has been autogenerated in the "js" directory.  You can use this to connect to your newly uploaded contract.

You will now see that Ether has been deducted from your account

![balance after](https://cloud.githubusercontent.com/assets/5578200/10926727/d91cc032-824e-11e5-928d-58574a94afbf.png)


Also, the newly created contract has been given its own address, which you can view in the data in the "meta" folder.  Viewing contract information, including compiled bytecode for your Solidity contract can be done using the same URL that you use to view your own account information.

![contract info](https://cloud.githubusercontent.com/assets/5578200/10926827/8a4fcb42-824f-11e5-883b-b4704797cc02.png)

## Running The Local Webserver

Bloc ships with a node server. To get the server up and running

```
bloc start
```

Now you can visit one of the contracts in your application, for example localhost:3000/contracts/payout

Bloc will run through 3 contract status checks
1) Does the contract exist in the project
2) Has the contract been compiled
3) Has the contract been uploaded to the network

This will be reflected in the application as well as at the terminal

![bloc upload](https://raw.githubusercontent.com/blockapps/bloc/readme-images/readme_img/bloc_start.png)


## Commands

```
Usage: bloc <command> (options)

Commands:
  init               start a new project
  compile            compile contracts in contract folder
  upload [contract]  upload contract to blockchain
  genkey             generate a new private key and fill it at the faucet
  start              launch a webserver for your application
  send               start prompt, transfer (amount*unit) to (address)

Options:
  -s, --scaffold     scaffold html / js / css from your contracts when compiling or
                     uploading
```

## Additional Resources
bloc uses [blockapps-js](https://github.com/blockapps/blockapps-js), our simple library for interfacing with the blockchain.
Smart contracts that are written in javascript-like language called [Solidity](https://github.com/ethereum/wiki/wiki/The-Solidity-Programming-Language). A good place to start playing around with Solidity is the [online compiler](https://chriseth.github.io/browser-solidity/).