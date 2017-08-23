
## Start the Ethereum node, connect to other peer nodes and start downloading the blockchain.

```sh
> geth --testnet --syncmode "fast" --rpc --rpcapi db,eth,net,web3,personal --cache=1024  --rpcport 8545 --rpcaddr 127.0.0.1 --rpccorsdomain "*" --bootnodes "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303,enode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303"

# You can mine some ether by running your geth node by passing an additional option --mine

geth --testnet --mine --syncmode "fast" --rpc --rpcapi db,eth,net,web3,personal --cache=1024  --rpcport 8545 --rpcaddr 127.0.0.1 --rpccorsdomain "*" --bootnodes "enode://20c9ad97c081d63397d7b685a412227a40e23c8bdc6688c6f37e97cfbc22d2b4d1db1510d8f61e6a8866ad7f0e17c02b14182d37ea7c3c8b9c2683aeb6b733a1@52.169.14.227:30303,enode://6ce05930c72abc632c58e2e4324f7c7ea478cec0ed4fa2528982cf34483094e9cbc9216e7aa349691242576d552a2a56aaeae426c5303ded677ce455ba1acd9d@13.84.180.240:30303" --unlock 0xb5180149e23bad1f9bfa53dfb0f52da40310bd07
```

## Install truffle & webpack

```sh
> npm install -g truffle webpack
> mkcd appName
> truffle init webpack
```

## Create addresses

```sh
# Before we can deploy the contract, we will need an account and some ether. When we used the testrpc, it created 10 test accounts and came preloaded with 100 test ethers. But for testnet and mainnet we have to create the account and add some ether ourselves.

> truffle console
> web3.personal.newAccount('verystrongpassword')
> web3.eth.getBalance('0x95a94979d86d9c32d1d2ab5ace2dcc8d1b446fa1')
> web3.personal.unlockAccount('0x95a94979d86d9c32d1d2ab5ace2dcc8d1b446fa1', 'verystrongpassword', 15000)

# Replace 'verystrongpassword' with a good strong password.
# The account is locked by default, make sure to unlock it before using the account for deploying and interacting with the blockchain.
# Meaning unlock the first account with this for 15,000 seconds (don't bug me for a while.)
```

##  Compile and deploy the contract to the blockchain
```sh
> truffle compile
> truffle migrate -- reset
> truffle migrate --network live / development
```

### results...
```sh
Using network 'development'.Running migration: 1_initial_migration.js  Replacing Migrations...  ... 0x30d05d9c4c95f8ea9e1b4a50de1178eb616cd3f4bbcfefc4362fa9ff23e9dfb3
  Migrations: 0xf54b473907762530247fe613ac4eb0f2ea7232fb
Saving successful migration to network...
  ... 0x5f87fc664adbfb81b3adb0279bcf95121dfca087e12221b2c5cae648ae26ea9a
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying Voting...
  ... 0x2746c758e67838a6729d3d05c2cd5e70467f05320bf039ac84a417d939c61f43
  Voting: 0x0066f6436f3b04667e1d2bfa6d559da5c1363d57
Saving successful migration to network...
  ... 0xa503d5fad7893e2aafecc21d3fe8766a19ad2b1e9bc2c1d14d75c809a5ddb93e
Saving artifacts...
```

### “Error: authentication needed: password or unlock” — Can you check and make sure you unlocked your account?

* use `web3.personal.unlockAccount` in truffle console or
* use `- — unlock` option when you run geth.
* make sure you're unlocking the right account

## Use Web3 to check accounts

```sh
> node
> web3 = require('web3')
> var web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
```

## Interacting with contract in truffle console

```sh
> Voting.deployed().then(function(contractInstance) {contractInstance.voteForCandidate('Rama').then(function(v) {console.log(v)})})

> Voting.deployed().then(function(contractInstance) {contractInstance.totalVotesFor.call('Rama').then(function(v) {console.log(v)})})
```

## Run Server

```sh
npm run dev
```

## List Geth Accounts

```sh
> geth account list
> geth --testnet account list
```