# Task 1: Create and deploy a simple, smart contract with cargo casper and cargo test

## Prerequisites:
###  1.1 Installing Rust

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

![](https://i.imgur.com/ZWUnoap.png)


Check version

`rustup --version`

![](https://i.imgur.com/Uvilfhb.png)


### 1.2 Installing Dependencies
#### 1.2.1 CMake

```
sudo apt install cmake
```

![](https://i.imgur.com/PjMvNFX.png)


Check Cmake version

`cmake --version`


### 1.3 Development Environment Setup
#### 1.3.1 Installing the Casper Crates

`cargo install cargo-casper`

![](https://i.imgur.com/G1Hx99Z.png)

#### 1.3.2 Creating a Project
`cargo casper my-casper-project
`

#### 1.3.3 Test the contract
`make prepare`

![](https://i.imgur.com/y0skOxz.png)

`make build-contract`

![](https://i.imgur.com/yKkqNi0.png)

`make test`

![](https://i.imgur.com/c67zB5X.png)


---------------

# Task 2: Complete one of the existing tutorials for writing smart contracts

I chose “A counter on the Casper chain” tutorial

## Prerequisites:

-	Set up a Casper Rust development environment
-	Set up a local network with NCTL
-	Install the casper-client


## Work plan:

![](https://i.imgur.com/6B4pPaU.png)

### 2.1 Set up a local network with NCTL
#### 2.1.1 Pip install
`sudo apt install python3-pip`

![](https://i.imgur.com/quBel8b.png)

#### 2.1.2 Install pkg-config
`sudo apt install pkg-config`

![](https://i.imgur.com/S38otC3.png)

#### 2.1.3 Install either libssl-dev (Linux)
`sudo apt install libssl-dev`

![](https://i.imgur.com/vqnrsjg.png)

#### 2.1.4  Install gcc and g++ compilers
```
sudo apt install build-essential
gcc --version
g++ --version
```

![](https://i.imgur.com/jBYwSSL.png)

#### 2.1.5 Create and activate a new virtual environment.

```
sudo apt-get install python3-venv

$ python3 -m venv env
$ source env/bin/activate
(env) $
```

![](https://i.imgur.com/ywHsFzY.png)

#### 2.1.6 Inside the virtual environment, upgrade pip to the latest version.
`(env) $ pip install --upgrade pip`

![](https://i.imgur.com/TxitKKL.png)

#### 2.1.7 Install jq, a command-line JSON processor.
``(env) $ pip install jq`
#### 2.1.8 Install supervisor, a cross-platform process manager.
``(env) $ pip install supervisor`
#### 2.1.9 Install toml, a configuration file parser.
``(env) $ pip install toml`


### 2.2 Setting up the Network
#### 2.2.1 Clone the casper-node-launcher software.
` (env) $ git clone https://github.com/casper-network/casper-node-launcher`

![](https://i.imgur.com/lniQo0U.png)

#### 2.2.2 Clone the casper-node software.
`(env) $ git clone https://github.com/casper-network/casper-node`

![](https://i.imgur.com/mUiSNRM.png)

#### 2.2.3 Activate the NCTL environment with the following command.
`(env) $ source casper-node/utils/nctl/activate`

#### 2.2.4 Compile the NCTL binary scripts
`(env) $ nctl-compile`

![](https://i.imgur.com/xTU5V2Q.png)

#### 2.2.5 Set up all the assets required to run a local network
`(env) $ nctl-assets-setup && nctl-start`

![](https://i.imgur.com/SDqb9gH.png)

### 2.3 Installing the Casper Client
```
rustup toolchain install nightly
cargo +nightly-2021-06-17 install casper-client --locked
```

![](https://i.imgur.com/b6FnviS.png)

### 2.4 Clone the Contracts
`git clone https://github.com/casper-ecosystem/counter`

### 2.5 View the Network State
#### 2.5.1 Get the account hash
`nctl-view-faucet-account`

![](https://i.imgur.com/S3phgf6.png)

#### 2.5.2  Get the state root hash
`casper-client get-state-root-hash --node-address http://localhost:11101`

![](https://i.imgur.com/gZIJ3vM.png)

#### 2.5.3 Get the network state
```
casper-client query-state 
    --node-address http://localhost:11101 
    --state-root-hash e2f6b86240b5ab5e8df9b76a931b8d4e33f7cdf324be117b7aea6798e69a2c17 
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de
```

![](https://i.imgur.com/ba9GjfP.png)


### 2.6 Deploy the Counter

```
make prepare
make test
```

![](https://i.imgur.com/Q9tkOwx.png)

![](https://i.imgur.com/IgdlOmN.png)

#### 2.6.1 Put the contract on the chain.
```
casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /root/casper-project/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-path ./casper-project/counter/target/wasm32-unknown-unknown/release/counter-define.wasm
```

![](https://i.imgur.com/NqBCryo.png)

#### 2.6.2 Verify that the deploy successfully took place.
```
casper-client get-deploy \
    --node-address http://localhost:11101 82ca63a86bbb3159cf09b076a3a99309d7e2333b9bc4520ed0464dc9b59acc65
```

![](https://i.imgur.com/AjKDkmf.png)

### 2.7 View the Updated Network State

#### 2.7.1 Get the NEW state-root-hash:
`casper-client get-state-root-hash --node-address http://localhost:11101`

![](https://i.imgur.com/2cVnrks.png)

#### 2.7.2 Get the network state:
```
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 41d90ebf37c493fd1894f4d3bd30031c93fb6934428d36582768f78e338c77ed \
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de
```

![](https://i.imgur.com/2gZdkri.png)

#### 2.7.3 Retrieve the specific counter contract details:

```
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 41d90ebf37c493fd1894f4d3bd30031c93fb6934428d36582768f78e338c77ed \
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de -q "counter"
```

![](https://i.imgur.com/r5gZW6h.png)

#### 2.7.4 Retrieve the specific counter variable details:
```
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 41d90ebf37c493fd1894f4d3bd30031c93fb6934428d36582768f78e338c77ed \
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de -q "counter/count"
```

![](https://i.imgur.com/CvbQVxa.png)

#### 2.7.5 Retrieve the specific deploy details:
```
casper-client query-state --node-address http://localhost:11101 \
    --state-root-hash 41d90ebf37c493fd1894f4d3bd30031c93fb6934428d36582768f78e338c77ed --key deploy-82ca63a86bbb3159cf09b076a3a99309d7e2333b9bc4520ed0464dc9b59acc65
```

![](https://i.imgur.com/WcZFpWY.png)

### 2.8 Increment the Counter
```
cd casper-project

casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /root/casper-project/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-name "counter" \
    --session-entry-point "counter_inc"
```

![](https://i.imgur.com/XS5h2WD.png)

### 2.9 View the Updated Network State Again
`casper-client get-state-root-hash --node-address http://localhost:11101`

### 2.10 Get the network state, specifically for the count variable this time:
```
casper-client query-state --node-address http://localhost:11101 \
    --state-root-hash 6587167db97f618d1c7d4a87fa29f79248b79e4b485ba68236e39ce4457a7d7c \
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de -q "counter/count"
```

![](https://i.imgur.com/FkeWgAl.png)

### 2.11 Increment the Counter Again (2nd option)

```
casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /root/casper-project/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-path /root/casper-project/counter/target/wasm32-unknown-unknown/release/counter-call.wasm
```

![](https://i.imgur.com/Eyl13hm.png)

### 2.12 View the Final Network State

`casper-client get-state-root-hash --node-address http://localhost:11101`

### 2.13 Get the network state, specifically for the count variable this time:

```
casper-client query-state --node-address http://localhost:11101 \
    --state-root-hash 81897d77074310ace9a98d7a6e8afa1ca6ccbef67579e55af30f44d80d0f1d1c \
    --key account-hash-b8dbc81b6b9efbad3ccb1cd6818a492d384f892737e04663e02d20d73bb3b8de -q "counter/count"
```

![](https://i.imgur.com/uHW4AN5.png)

------------

# Task 3: Demonstrate key management concepts by modifying the client in the Multi-Sig tutorial to address one of the additional scenarios

## 3.1 Implementing the Smart Contract

First, download the example contract and client for key management.

`git clone https://github.com/casper-ecosystem/keys-manager`

![](https://i.imgur.com/L5TYRTS.png)

Compile the smart contract and create the WASM file using these commands:

```
cd contract
cargo build --release
```

![](https://i.imgur.com/UmAX0Hd.png)

## 3.2 Setting up a local Casper Network
`nctl-assets-setup && nctl-start`

![](https://i.imgur.com/fhOvQWW.png)

See faucet account details 
`nctl-view-faucet-account`

![](https://i.imgur.com/lsjON2J.png)

## 3.3 Setting up the Client
```
cd keys-manager/client/
touch .env
nano -e .env
```

![](https://i.imgur.com/VvKaXNI.png)

Install the JavaScript packages in the keys-manager/client

`npm install`

![](https://i.imgur.com/XvDjkZ4.png)

## 3.4 Testing the Client
`npm run start:atomic`

Need to wait some time after entering the above command until we see a result.	

![](https://i.imgur.com/LBbc1L3.png)

## Scenario 2: deploying with special keys

In this example, you have two keys. One key can only perform deployments, while the second key can perform key management and deployments. The key with account address a1 can deploy and make account changes, but the second key with account address b2 can only deploy.

![](https://i.imgur.com/4dSGM6F.png)

### Code of scenario nr. 2

I wrote this code for the selected scenario:

```
const keyManager = require('./key-manager');
const TRANSFER_AMOUNT = process.env.TRANSFER_AMOUNT || 2500000000;

(async function () {

    // Scenario 2: deploying with special keys
    // In this example, we have two keys:
    // - First key (mainAccount) can only perform deployments, 
    // - The second key can perform key management and deployments.

    // To achive the task, we will:
    // 1. Set mainAccount's weight to 2.
    // 2. Set Keys Management Threshold to 2.
    // 3. Add new key with weight 1 (The second account).
    // 4. Make a transfer from mainAccount.
    // 5. Make a transfer from mainAccount using the second account.

    let deploy;

    let masterKey = keyManager.randomMasterKey();
    let mainAccount = masterKey.deriveIndex(1);
    let secondAccount = masterKey.deriveIndex(2);

    console.log("\n0.1 Fund main account.\n");
    await keyManager.fundAccount(mainAccount);
    await keyManager.printAccount(mainAccount);
    
    console.log("\n[x]0.2 Install Keys Manager contract");
    deploy = keyManager.keys.buildContractInstallDeploy(mainAccount);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);

    // 1. Set mainAccount's weight to 2
    console.log("\n1. Set faucet's weight to 2\n");
    deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, mainAccount, 2);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);
    
    // 2. Set Keys Management Threshold to 2.
    console.log("\n2. Set Keys Management Threshold to 2\n");
    deploy = keyManager.keys.setKeyManagementThresholdDeploy(mainAccount, 2);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);
    
    // 3. Add new key with weight 1 (The second account).
    console.log("\n4. Add new key with weight 1 (The second account).\n");
    deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, secondAccount, 1);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);
    
    // 4. Make a transfer from mainAccount.
    console.log("\n6. Make a transfer from mainAccount.\n");
    deploy = keyManager.transferDeploy(mainAccount, secondAccount, TRANSFER_AMOUNT);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);

    // 5. Make a transfer from mainAccount using the second account.
    console.log("\n6. Make a transfer from mainAccount using the second account.\n");
    deploy = keyManager.transferDeploy(mainAccount, secondAccount, TRANSFER_AMOUNT);
    await keyManager.sendDeploy(deploy, [secondAccount]);
    await keyManager.printAccount(mainAccount);
    
    
})();


```

### Result: 

![](https://i.imgur.com/2jCMR7E.png)


-------------

# Task 4: Learn to transfer tokens to an account on the Casper Testnet.

## Prerequisites: 

![](https://i.imgur.com/yetinuM.png)

## 4.1 Setting up an Account
`casper-client keygen .`

![](https://i.imgur.com/oznxGDc.png)

![](https://i.imgur.com/PZWJbO9.png)

## 4.2 Fund account from Faucet

https://testnet.cspr.live/tools/faucet

![](https://i.imgur.com/0ROY0oM.png)

## 4.3 Acquire Node Address from network peers

https://testnet.cspr.live/tools/peers

`http://164.90.198.193:7777`

## 4.4 Transfer

```
casper-client transfer \
    --id 1 \
    --transfer-id 0 \
    --node-address http://164.90.198.193:7777 \
    --amount 7700000000 \
    --payment-amount 10000 \
    --secret-key secret_key.pem \
    --chain-name casper-test \
    --target-account 01117809959ead668a82df3cc0ad1737db1682ab63d180973b0ed7002675241d0c
```

![](https://i.imgur.com/wKGISfd.png)

![](https://i.imgur.com/3fwTtTI.png)


-------

# Task 5: Learn to Delegate and Undelegate on the Casper Testnet.

## Delegate:

![](https://i.imgur.com/rQfLFaW.png)

![](https://i.imgur.com/HgJwPmk.png)


## Undelegate

![](https://i.imgur.com/em5XAjh.png)

