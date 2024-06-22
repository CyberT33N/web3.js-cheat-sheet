# web3.js Cheat Sheet
- https://www.npmjs.com/package/web3
- https://github.com/web3/web3.js
- https://web3js.org/

<br><br>
<br><br>

# Docs
- https://docs.web3js.org/guides/getting_started/quickstart












<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Initialize Web3 with a provider
```javascript
import { Web3 } from 'web3';

//private RPC endpoint 
const web3 = new Web3('https://mainnet.infura.io/v3/API_KEY'); 

//or public RPC endpoint
//const web3 = new Web3('https://eth.llamarpc.com'); 

web3.eth.getBlockNumber().then(console.log);
// ↳ 18849658n
```


















<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Wallet
- https://docs.web3js.org/libdocs/Wallet/

<details><summary>Click to expand..</summary>
  
- A Web3.js Wallet is your main entry point if you want to use a private key directly to do any blockchain operations (transactions), also called Signer in other libraries.

Unlike other libraries where a wallet holds just one account, a Web3.js Wallet can handle multiple accounts. They each have their private key and address. So, whether those keys are in your computer's memory or protected by MetaMask, the Wallet makes Ethereum tasks secure and simple.

The web3-eth-accounts package contains functions to generate Ethereum accounts and sign transactions and data.

In Ethereum, a private key is a critical part of the cryptographic key pair used to secure and control ownership of Ethereum addresses. Each Ethereum address has a matching set of public and private keys in a public-key cryptography system. This key pair enables you to own an Ethereum address, manage funds, and initiate transactions.



<br><br>
<br><br>

## Wallets vs Accounts
- An account in web3.js is an object, it refers to an individual Ethereum address with its associated public and private keys. While a wallet is a higher-level construct for managing multiple accounts, an individual Ethereum address is considered an account.
- A wallet in web3.js is an array that holds multiple Ethereum accounts. It provides a convenient way to manage and interact with a collection of accounts. Think of it as a digital wallet that you use to store and organize your various Ethereum addresses.

![Web3.js Logo](https://docs.web3js.org/assets/images/image-365636f2b3d9af5957f2bf59aa1d79c4.jpeg)





<br><br>
<br><br>


## Local Wallet
- https://docs.web3js.org/guides/wallet/local_wallet/
- Local wallets are an in-memory wallet that can hold multiple accounts. Wallets are a convenient way to sign and send transactions in web3.js






<br><br>
<br><br>
<br><br>
<br><br>

## Create a Wallet with a random account
- https://docs.web3js.org/guides/getting_started/quickstart#create-random-wallet
```javascript
//create random wallet with 1 account
web3.eth.accounts.wallet.create(1)
/* ↳ 
Wallet(1) 
[
  {
    address: '0xcE6A5235d6033341972782a15289277E85E5b305',
    privateKey: '0x50d349f5cf627d44858d6fcb6fbf15d27457d35c58ba2d5cfeaf455f25db5bec',
    signTransaction: [Function: signTransaction],
    sign: [Function: sign],
    encrypt: [Function: encrypt]
  },
  _accountProvider: {
    create: [Function: createWithContext],
    privateKeyToAccount: [Function: privateKeyToAccountWithContext],
    decrypt: [Function: decryptWithContext]
  },
  _addressMap: Map(1) { '0xce6a5235d6033341972782a15289277e85e5b305' => 0 },
  _defaultKeyName: 'web3js_wallet'
]
*/
```


<br><br>
<br><br>

## Create an account and add it to an empty Wallet
```javascript
import { Web3 } from "web3";

const web3 = new Web3(/* PROVIDER */);

// 1st - creating a new account
const account = web3.eth.accounts.create();
/* ↳
{
  address: '0x0770B4713B62E0c08C43743bCFcfBAA39Fa703EF',
  privateKey: '0x97b0c07e275a0d8d9983331ca1a7ecb1a4a6f7dcdd7657529fe07446fa4dfe23',
  signTransaction: [Function: signTransaction],
  sign: [Function: sign],
  encrypt: [Function: encrypt]
}
*/

// 2nd - add the account to the wallet
const wallet = web3.eth.accounts.wallet.add(account);
/* ↳
Wallet(1) [
  {
    address: '0x0770B4713B62E0c08C43743bCFcfBAA39Fa703EF',
    privateKey: '0x97b0c07e275a0d8d9983331ca1a7ecb1a4a6f7dcdd7657529fe07446fa4dfe23',
    signTransaction: [Function: signTransaction],
    sign: [Function: sign],
    encrypt: [Function: encrypt]
  },
  _accountProvider: {
    create: [Function: createWithContext],
    privateKeyToAccount: [Function: privateKeyToAccountWithContext],
    decrypt: [Function: decryptWithContext]
  },
  _addressMap: Map(1) { '0x0770b4713b62e0c08c43743bcfcfbaa39fa703ef' => 0 },
  _defaultKeyName: 'web3js_wallet'
]
*/
```




<br><br>
<br><br>

## Add a private key to create a wallet
- https://docs.web3js.org/guides/getting_started/quickstart#add-a-private-key-to-create-a-wallet
```javascript
//the private key must start with the '0x' prefix
const account = web3.eth.accounts.wallet.add('0x50d349f5cf627d44858d6fcb6fbf15d27457d35c58ba2d5cfeaf455f25db5bec');

console.log(account[0].address);
//↳ 0xcE6A5235d6033341972782a15289277E85E5b305

console.log(account[0].privateKey);
//↳ 0x50d349f5cf627d44858d6fcb6fbf15d27457d35c58ba2d5cfeaf455f25db5bec
```









<br><br>
<br><br>

--  --  --  --  --  --  --  --  

<br><br>
<br><br>

## Transactions

<br><br>
<br><br>

### Send transactions
- https://docs.web3js.org/guides/wallet/#sending-transactions
- The shortest way to do this, is by creating a Wallet directly by adding a private key (the private key must start with '0x' and it must have funds to execute the transaction)
```javascript
import { Web3 } from 'web3';

const web3 = new Web3('https://ethereum-sepolia.publicnode.com');

//this will create an array `Wallet` with 1 account with this privateKey
//it will generate automatically a public key for it
//make sure you have funds in this accounts
const wallet = web3.eth.accounts.wallet.add('0x152c39c430806985e4dc16fa1d7d87f90a7a1d0a6b3f17efe5158086815652e5');

const _to = '0xc7203efeb54846c149f2c79b715a8927f7334e74';
const _value = '1'; //1 wei

//the `from` address in the transaction must match the address stored in our `Wallet` array
//that's why we explicitly access it using `wallet[0].address` to ensure accuracy
const receipt = await web3.eth.sendTransaction({
  from: wallet[0].address,
  to: _to,
  value: _value,
});
//if you have more than 1 account, you can change the address by accessing to another account
//e.g, `from: wallet[1].address`

console.log('Tx receipt:', receipt);
/* ↳
Tx receipt: {
  blockHash: '0xa43b43b6e13ba47f2283b4afc15271ba07d1bba0430bd0c430f770ba7c98d054',
  blockNumber: 4960689n,
  cumulativeGasUsed: 7055436n,
  effectiveGasPrice: 51964659212n,
  from: '0xa3286628134bad128faeef82f44e99aa64085c94',
  gasUsed: 21000n,
  logs: [],
  logsBloom: '0x00000...00000000',
  status: 1n,
  to: '0xc7203efeb54846c149f2c79b715a8927f7334e74',
  transactionHash: '0xb88f3f300f1a168beb3a687abc2d14c389ac9709f18b768c90792c7faef0de7c',
  transactionIndex: 41n,
  type: 2n
}
*/
```














<br><br>
<br><br>

--  --  --  --  --  --  --  --  

<br><br>
<br><br>

## Wallet Methods
- The following is a list of Wallet methods in the web3.eth.accounts.wallet package with description and example usage:
- [add](/libdocs/Wallet#add)
- [clear](/libdocs/Wallet#clear)
- [create](/libdocs/Wallet#create)
- [decrypt](/libdocs/Wallet#decrypt)
- [encrypt](/libdocs/Wallet#encrypt)
- [get](/libdocs/Wallet#get)
- [load](/libdocs/Wallet#load)
- [remove](/libdocs/Wallet#remove)
- [save](/libdocs/Wallet#save)
- [getStorage](/libdocs/Wallet#getStorage)

</details>


































<br><br>
<br><br>

______________________________________________________
______________________________________________________

<br><br>
<br><br>

# Acounts

<details><summary>Click to expand..</summary>

## Account Methods
- [create](https://docs.web3js.org/libdocs/Accounts#create)
- [decrypt](https://docs.web3js.org/libdocs/Accounts#decrypt)
- [encrypt](https://docs.web3js.org/libdocs/Accounts#encrypt)
- [hashMessage](https://docs.web3js.org/libdocs/Accounts#hashMessage)
- [parseAndValidatePrivateKey](https://docs.web3js.org/libdocs/Accounts#parseandvalidateprivatekey)
- [privateKeyToAccount](https://docs.web3js.org/libdocs/Accounts#privatekeytoaccount)
- [privateKeyToAddress](https://docs.web3js.org/libdocs/Accounts#privatekeytoaddress)
- [privateKeyToPublicKey](https://docs.web3js.org/libdocs/Accounts#privatekeytopublickey)
- [recover](https://docs.web3js.org/libdocs/Accounts#recover)
- [recoverTransaction](https://docs.web3js.org/libdocs/Accounts#recovertransaction)
- [sign](https://docs.web3js.org/libdocs/Accounts#sign)
- [signTransaction](https://docs.web3js.org/libdocs/Accounts#signtransaction)





</details>

























































<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Metamask

## React
```javascript
import { useState } from 'react';
import { Web3 } from 'web3';

function App() {
  //state to store and show the connected account
  const [connectedAccount, setConnectedAccount] = useState('null');
    
  async function connectMetamask() {
    //check metamask is installed
    if (window.ethereum) {
      // instantiate Web3 with the injected provider
      const web3 = new Web3(window.ethereum);

      //request user to connect accounts (Metamask will prompt)
      await window.ethereum.request({ method: 'eth_requestAccounts' });

      //get the connected accounts
      const accounts = await web3.eth.getAccounts();

      //show the first connected account in the react page
      setConnectedAccount(accounts[0]);
    } else {
      alert('Please download metamask');
    }
  }

  return (
    <>
      {/* Button to trigger Metamask connection */}
      <button onClick={() => connectMetamask()}>Connect to Metamask</button>

      {/* Display the connected account */}
      <h2>{connectedAccount}</h2>
    </>
  );
}

export default App;
```



























<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# web3-eth-accounts
- https://docs.web3js.org/api/web3-eth-accounts
- 
<details><summary>Click to expand..</summary>
  
## Methods
- [assertIsUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/assertIsUint8Array)
- [bigIntToHex](https://docs.web3js.org/api/web3-eth-accounts/function/bigIntToHex)
- [bigIntToUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/bigIntToUint8Array)
- [bigIntToUnpaddedUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/bigIntToUnpaddedUint8Array)
- [create](https://docs.web3js.org/api/web3-eth-accounts/function/create)
- [decrypt](https://docs.web3js.org/api/web3-eth-accounts/function/decrypt)
- [ecrecover](https://docs.web3js.org/api/web3-eth-accounts/function/ecrecover)
- [encrypt](https://docs.web3js.org/api/web3-eth-accounts/function/encrypt)
- [hashMessage](https://docs.web3js.org/api/web3-eth-accounts/function/hashMessage)
- [intToUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/intToUint8Array)
- [isAccessList](https://docs.web3js.org/api/web3-eth-accounts/function/isAccessList)
- [isAccessListUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/isAccessListUint8Array)
- [padToEven](https://docs.web3js.org/api/web3-eth-accounts/function/padToEven)
- [parseAndValidatePrivateKey](https://docs.web3js.org/api/web3-eth-accounts/function/parseAndValidatePrivateKey)
- [parseGethGenesis](https://docs.web3js.org/api/web3-eth-accounts/function/parseGethGenesis)
- [privateKeyToAccount](https://docs.web3js.org/api/web3-eth-accounts/function/privateKeyToAccount)
- [privateKeyToAddress](https://docs.web3js.org/api/web3-eth-accounts/function/privateKeyToAddress)
- [privateKeyToPublicKey](https://docs.web3js.org/api/web3-eth-accounts/function/privateKeyToPublicKey)
- [recover](https://docs.web3js.org/api/web3-eth-accounts/function/recover)
- [recoverTransaction](https://docs.web3js.org/api/web3-eth-accounts/function/recoverTransaction)
- [setLengthLeft](https://docs.web3js.org/api/web3-eth-accounts/function/setLengthLeft)
- [sign](https://docs.web3js.org/api/web3-eth-accounts/function/sign)
- [signTransaction](https://docs.web3js.org/api/web3-eth-accounts/function/signTransaction)
- [stripHexPrefix](https://docs.web3js.org/api/web3-eth-accounts/function/stripHexPrefix)
- [stripZeros](https://docs.web3js.org/api/web3-eth-accounts/function/stripZeros)
- [toType](https://docs.web3js.org/api/web3-eth-accounts/function/toType)
- [toUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/toUint8Array)
- [uint8ArrayToBigInt](https://docs.web3js.org/api/web3-eth-accounts/function/uint8ArrayToBigInt)
- [unpadUint8Array](https://docs.web3js.org/api/web3-eth-accounts/function/unpadUint8Array)
- [zeros](https://docs.web3js.org/api/web3-eth-accounts/function/zeros)

</details>

























<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Classes
- https://docs.web3js.org/api/web3/class
















<br><br>
<br><br>
<br><br>
<br><br>


## Contract
- The web3.eth.Contract makes it easy to interact with smart contracts on the ethereum blockchain
- https://docs.web3js.org/api/web3/class/Contract

<details><summary>Click to expand..</summary>

<br><br>

### Acessors
- [BatchRequest](https://docs.web3js.org/api/web3/class/Contract#BatchRequest)
- [accountProvider](https://docs.web3js.org/api/web3/class/Contract#accountProvider)
- [blockHeaderTimeout](https://docs.web3js.org/api/web3/class/Contract#blockHeaderTimeout)
- [contractDataInputFill](https://docs.web3js.org/api/web3/class/Contract#contractDataInputFill)
- [currentProvider](https://docs.web3js.org/api/web3/class/Contract#currentProvider)
- [defaultAccount](https://docs.web3js.org/api/web3/class/Contract#defaultAccount)
- [defaultBlock](https://docs.web3js.org/api/web3/class/Contract#defaultBlock)
- [defaultChain](https://docs.web3js.org/api/web3/class/Contract#defaultChain)
- [defaultCommon](https://docs.web3js.org/api/web3/class/Contract#defaultCommon)
- [defaultHardfork](https://docs.web3js.org/api/web3/class/Contract#defaultHardfork)
- [defaultMaxPriorityFeePerGas](https://docs.web3js.org/api/web3/class/Contract#defaultMaxPriorityFeePerGas)
- [defaultNetworkId](https://docs.web3js.org/api/web3/class/Contract#defaultNetworkId)
- [defaultReturnFormat](https://docs.web3js.org/api/web3/class/Contract#defaultReturnFormat)
- [defaultTransactionType](https://docs.web3js.org/api/web3/class/Contract#defaultTransactionType)
- [enableExperimentalFeatures](https://docs.web3js.org/api/web3/class/Contract#enableExperimentalFeatures)
- [events](https://docs.web3js.org/api/web3/class/Contract#events)
- [givenProvider](https://docs.web3js.org/api/web3/class/Contract#givenProvider)
- [handleRevert](https://docs.web3js.org/api/web3/class/Contract#handleRevert)
- [maxListenersWarningThreshold](https://docs.web3js.org/api/web3/class/Contract#maxListenersWarningThreshold)
- [methods](https://docs.web3js.org/api/web3/class/Contract#methods)
- [provider](https://docs.web3js.org/api/web3/class/Contract#provider)
- [requestManager](https://docs.web3js.org/api/web3/class/Contract#requestManager)
- [subscriptionManager](https://docs.web3js.org/api/web3/class/Contract#subscriptionManager)
- [transactionBlockTimeout](https://docs.web3js.org/api/web3/class/Contract#transactionBlockTimeout)
- [transactionBuilder](https://docs.web3js.org/api/web3/class/Contract#transactionBuilder)
- [transactionConfirmationBlocks](https://docs.web3js.org/api/web3/class/Contract#transactionConfirmationBlocks)
- [transactionConfirmationPollingInterval](https://docs.web3js.org/api/web3/class/Contract#transactionConfirmationPollingInterval)
- [transactionPollingInterval](https://docs.web3js.org/api/web3/class/Contract#transactionPollingInterval)
- [transactionPollingTimeout](https://docs.web3js.org/api/web3/class/Contract#transactionPollingTimeout)
- [transactionReceiptPollingInterval](https://docs.web3js.org/api/web3/class/Contract#transactionReceiptPollingInterval)
- [transactionSendTimeout](https://docs.web3js.org/api/web3/class/Contract#transactionSendTimeout)
- [transactionTypeParser](https://docs.web3js.org/api/web3/class/Contract#transactionTypeParser)
- [wallet](https://docs.web3js.org/api/web3/class/Contract#wallet)

<br><br>

## Methods
- [clone](https://docs.web3js.org/api/web3/class/Contract#clone)
- [decodeMethodData](https://docs.web3js.org/api/web3/class/Contract#decodeMethodData)
- [deploy](https://docs.web3js.org/api/web3/class/Contract#deploy)
- [emit](https://docs.web3js.org/api/web3/class/Contract#emit)
- [eventNames](https://docs.web3js.org/api/web3/class/Contract#eventNames)
- [extend](https://docs.web3js.org/api/web3/class/Contract#extend)
- [getContextObject](https://docs.web3js.org/api/web3/class/Contract#getContextObject)
- [getMaxListeners](https://docs.web3js.org/api/web3/class/Contract#getMaxListeners)
- [getPastEvents](https://docs.web3js.org/api/web3/class/Contract#getPastEvents)
- [link](https://docs.web3js.org/api/web3/class/Contract#link)
- [listenerCount](https://docs.web3js.org/api/web3/class/Contract#listenerCount)
- [listeners](https://docs.web3js.org/api/web3/class/Contract#listeners)
- [off](https://docs.web3js.org/api/web3/class/Contract#off)
- [on](https://docs.web3js.org/api/web3/class/Contract#on)
- [once](https://docs.web3js.org/api/web3/class/Contract#once)
- [registerPlugin](https://docs.web3js.org/api/web3/class/Contract#registerPlugin)
- [removeAllListeners](https://docs.web3js.org/api/web3/class/Contract#removeAllListeners)
- [setConfig](https://docs.web3js.org/api/web3/class/Contract#setConfig)
- [setMaxListenerWarningThreshold](https://docs.web3js.org/api/web3/class/Contract#setMaxListenerWarningThreshold)
- [setProvider](https://docs.web3js.org/api/web3/class/Contract#setProvider)
- [setRequestManagerMiddleware](https://docs.web3js.org/api/web3/class/Contract#setRequestManagerMiddleware)
- [use](https://docs.web3js.org/api/web3/class/Contract#use)
- [fromContextObject](https://docs.web3js.org/api/web3/class/Contract#fromContextObject)



<br><br>
<br><br>

## Instantiate a contract
```javascript
//Uniswap token address in mainnet
const address = '0x1f9840a85d5af5bf1d1762f925bdaddc4201f984'

//you can find the complete ABI in etherscan.io
const ABI = 
[
    {
      name: 'symbol',
      outputs: [{ type: 'string' }],
      type: 'function',
    },
    {
      name: 'totalSupply',
      outputs: [{ type: 'uint256' }],
      type: 'function',
    },
];

//instantiate the contract
const uniswapToken = new web3.eth.Contract(abi, address);
```

<br><br>
<br><br>

## Read-methods
```javascript
//make the call to the contract
const symbol = await uniswapToken.methods.symbol().call();

console.log('Uniswap symbol:',symbol);
// ↳ Uniswap symbol: UNI

//make the call to the contract
const totalSupply = await uniswapToken.methods.totalSupply().call();

console.log('Uniswap Total supply:', totalSupply);
// ↳ Uniswap Total Supply: 1000000000000000000000000000n

//use web3 utils to format the units
console.log(web3.utils.fromWei(totalSupply, 'ether'))
// ↳ 1000000000
```



<br><br>
<br><br>


## Writing-methods
```javascript
//address to send the token
const to = '0xcf185f2F3Fe19D82bFdcee59E3330FD7ba5f27ce';

//value to transfer (1 with 18 decimals)
const value = web3.utils.toWei('1','ether');

//send the transaction => return the Tx receipt
const txReceipt = await uniswapToken.methods.transfer(to,value).send({from: account[0].address});

console.log('Tx hash:',txReceipt.transactionHash);
// ↳ Tx hash: 0x14273c2b5781cc8f1687906c68bfc93482c603026d01b4fd37a04adb6217ad43
```



<br><br>
<br><br
-- -- -- -- -- -- -- -- -- -- -- 
<br><br>
<br><br>



## getPastEvents
```javascript
//get past `Transfer` events from block 18850576
const eventTransfer = await uniswapToken.getPastEvents('Transfer', { fromBlock: 18850576 });

console.log(eventTransfer);
// ↳ [{...},{...}, ...] array with all the events emitted
//you can only query logs from the previous 100_000 blocks 
```

### get past created pairs
```javascript
const UNISWAP_FACTORY_ADDRESS = "0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f"; // Mainnet UniswapV2Factory
const FACTORY_ABI = [
  {
    "anonymous": false,
    "inputs": [
      {
        "indexed": true,
        "internalType": "address",
        "name": "token0",
        "type": "address"
      },
      {
        "indexed": true,
        "internalType": "address",
        "name": "token1",
        "type": "address"
      },
      {
        "indexed": false,
        "internalType": "address",
        "name": "pair",
        "type": "address"
      },
      {
        "indexed": false,
        "internalType": "uint256",
        "name": "undefined",
        "type": "uint256"
      }
    ],
    "name": "PairCreated",
    "type": "event"
  }
];

// Create a contract instance
const uniswapFactory = new web3.eth.Contract(FACTORY_ABI, UNISWAP_FACTORY_ADDRESS);

const pairs = await uniswapFactory.getPastEvents('PairCreated', {
  fromBlock: 	20093620,
  toBlock: 	20093630
})
```








<br><br>
<br><br>
<br><br>
<br><br>


## Events
- https://docs.web3js.org/api/web3/class/Contract#events

<br><br>
<br><br>

### Track new pairs in uniswap
- token0 is the layer 2 coin
- token1: Die Adresse des zweiten Tokens im neuen Liquiditätspaar.
- pair: Die Adresse des neu erstellten Liquiditätspaares auf Uniswap, welches aus token0 und token1 besteht.
```
const UNISWAP_FACTORY_ADDRESS = "0x5C69bEe701ef814a2B6a3EDD4B1652CB9cc5aA6f";
const FACTORY_ABI = [
  {
    "anonymous": false,
    "inputs": [
      {
        "indexed": true,
        "internalType": "address",
        "name": "token0",
        "type": "address"
      },
      {
        "indexed": true,
        "internalType": "address",
        "name": "token1",
        "type": "address"
      },
      {
        "indexed": false,
        "internalType": "address",
        "name": "pair",
        "type": "address"
      },
      {
        "indexed": false,
        "internalType": "uint256",
        "name": "undefined",
        "type": "uint256"
      }
    ],
    "name": "PairCreated",
    "type": "event"
  }
];

// Create a contract instance
const uniswapFactory = new web3.eth.Contract(FACTORY_ABI, UNISWAP_FACTORY_ADDRESS);

// Subscribe to PairCreated events
const myEvent = await uniswapFactory.events.PairCreated()

myEvent.on('data', (event) => {
  console.log(`New pair created! Token0: ${event.returnValues.token0}, Token1: ${event.returnValues.token1}, Pair Address: ${event.returnValues.pair}`);
})

myEvent.on('error', (error) => {
  console.error(error);
})
```





<br><br>
<br><br>

### Get details of layer-2 coin
```javascript
// ERC20 ABI (Standard ABI für ERC20 Tokens)
const ERC20_ABI = [
  {
    "constant": true,
    "inputs": [],
    "name": "name",
    "outputs": [{"name": "","type": "string"}],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "symbol",
    "outputs": [{"name": "","type": "string"}],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "decimals",
    "outputs": [{"name": "","type": "uint8"}],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  },
  {
    "constant": true,
    "inputs": [],
    "name": "totalSupply",
    "outputs": [{"name": "","type": "uint256"}],
    "payable": false,
    "stateMutability": "view",
    "type": "function"
  }
];


// Replace with the ERC20 token address you want to query
const tokenAddress = "0x69352631646fB9471F84aeb31E2A15bf7c069868";

async function getTokenDetails() {
  try {
    const tokenContract = new web3.eth.Contract(ERC20_ABI, tokenAddress);

    // Fetch token details
    const [name, symbol, decimals, totalSupply] = await Promise.all([
      tokenContract.methods.name().call(),
      tokenContract.methods.symbol().call(),
      tokenContract.methods.decimals().call(),
      tokenContract.methods.totalSupply().call()
    ]);

    console.log(`Token Name: ${name}`);
    console.log(`Token Symbol: ${symbol}`);
    console.log(`Decimals: ${decimals}`);
    console.log(`Total Supply: ${totalSupply}`);
  } catch (error) {
    console.error("Error fetching token details:", error);
  }
}

getTokenDetails();
```
- Decimals (Dezimalstellen): Die Anzahl der Dezimalstellen, die verwendet werden, um den Token-Betrag darzustellen. Im Code ist es als 18n definiert, was darauf hinweist, dass dieser Token 18 Dezimalstellen unterstützt, was typisch für viele ERC-20 Tokens ist.

- TotalSupply (Gesamtangebot): Die Gesamtmenge des Tokens, die im Umlauf ist. Im Beispiel ist es als 1000000000000000000000000000n definiert. Das Suffix n steht für eine BigInt-Zahl in JavaScript, was bedeutet, dass es sich um eine sehr große Ganzzahl handelt. In diesem Fall repräsentiert es eine Gesamtversorgung von 1 Trillion Einheiten des Tokens.





</details>





















































<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>


# Web3Eth
- https://docs.web3js.org/api/web3-eth
- The Web3Eth allows you to interact with an Ethereum blockchain.

<details><summary>Click to expand..</summary>
  
<br><br>

## Methods
- [calculateFeeData](https://docs.web3js.org/api/web3/class/Web3Eth/#calculateFeeData)
- [call](https://docs.web3js.org/api/web3/class/Web3Eth/#call)
- [clearSubscriptions](https://docs.web3js.org/api/web3/class/Web3Eth/#clearSubscriptions)
- [createAccessList](https://docs.web3js.org/api/web3/class/Web3Eth/#createAccessList)
- [emit](https://docs.web3js.org/api/web3/class/Web3Eth/#emit)
- [estimateGas](https://docs.web3js.org/api/web3/class/Web3Eth/#estimateGas)
- [eventNames](https://docs.web3js.org/api/web3/class/Web3Eth/#eventNames)
- [extend](https://docs.web3js.org/api/web3/class/Web3Eth/#extend)
- [getAccounts](https://docs.web3js.org/api/web3/class/Web3Eth/#getAccounts)
- [getBalance](https://docs.web3js.org/api/web3/class/Web3Eth/#getBalance)
- [getBlock](https://docs.web3js.org/api/web3/class/Web3Eth/#getBlock)
- [getBlockNumber](https://docs.web3js.org/api/web3/class/Web3Eth/#getBlockNumber)
- [getBlockTransactionCount](https://docs.web3js.org/api/web3/class/Web3Eth/#getBlockTransactionCount)
- [getBlockUncleCount](https://docs.web3js.org/api/web3/class/Web3Eth/#getBlockUncleCount)
- [getChainId](https://docs.web3js.org/api/web3/class/Web3Eth/#getChainId)
- [getCode](https://docs.web3js.org/api/web3/class/Web3Eth/#getCode)
- [getCoinbase](https://docs.web3js.org/api/web3/class/Web3Eth/#getCoinbase)
- [getContextObject](https://docs.web3js.org/api/web3/class/Web3Eth/#getContextObject)
- [getFeeHistory](https://docs.web3js.org/api/web3/class/Web3Eth/#getFeeHistory)
- [getGasPrice](https://docs.web3js.org/api/web3/class/Web3Eth/#getGasPrice)
- [getHashRate](https://docs.web3js.org/api/web3/class/Web3Eth/#getHashRate)
- [getHashrate](https://docs.web3js.org/api/web3/class/Web3Eth/#getHashrate)
- [getMaxListeners](https://docs.web3js.org/api/web3/class/Web3Eth/#getMaxListeners)
- [getMaxPriorityFeePerGas](https://docs.web3js.org/api/web3/class/Web3Eth/#getMaxPriorityFeePerGas)
- [getNodeInfo](https://docs.web3js.org/api/web3/class/Web3Eth/#getNodeInfo)
- [getPastLogs](https://docs.web3js.org/api/web3/class/Web3Eth/#getPastLogs)
- [getPendingTransactions](https://docs.web3js.org/api/web3/class/Web3Eth/#getPendingTransactions)
- [getProof](https://docs.web3js.org/api/web3/class/Web3Eth/#getProof)
- [getProtocolVersion](https://docs.web3js.org/api/web3/class/Web3Eth/#getProtocolVersion)
- [getStorageAt](https://docs.web3js.org/api/web3/class/Web3Eth/#getStorageAt)
- [getTransaction](https://docs.web3js.org/api/web3/class/Web3Eth/#getTransaction)
- [getTransactionCount](https://docs.web3js.org/api/web3/class/Web3Eth/#getTransactionCount)
- [getTransactionFromBlock](https://docs.web3js.org/api/web3/class/Web3Eth/#getTransactionFromBlock)
- [getTransactionReceipt](https://docs.web3js.org/api/web3/class/Web3Eth/#getTransactionReceipt)
- [getUncle](https://docs.web3js.org/api/web3/class/Web3Eth/#getUncle)
- [getWork](https://docs.web3js.org/api/web3/class/Web3Eth/#getWork)
- [isMining](https://docs.web3js.org/api/web3/class/Web3Eth/#isMining)
- [isSyncing](https://docs.web3js.org/api/web3/class/Web3Eth/#isSyncing)
- [link](https://docs.web3js.org/api/web3/class/Web3Eth/#link)
- [listenerCount](https://docs.web3js.org/api/web3/class/Web3Eth/#listenerCount)
- [listeners](https://docs.web3js.org/api/web3/class/Web3Eth/#listeners)
- [off](https://docs.web3js.org/api/web3/class/Web3Eth/#off)
- [on](https://docs.web3js.org/api/web3/class/Web3Eth/#on)
- [once](https://docs.web3js.org/api/web3/class/Web3Eth/#once)
- [registerPlugin](https://docs.web3js.org/api/web3/class/Web3Eth/#registerPlugin)
- [removeAllListeners](https://docs.web3js.org/api/web3/class/Web3Eth/#removeAllListeners)
- [requestAccounts](https://docs.web3js.org/api/web3/class/Web3Eth/#requestAccounts)
- [sendSignedTransaction](https://docs.web3js.org/api/web3/class/Web3Eth/#sendSignedTransaction)
- [sendTransaction](https://docs.web3js.org/api/web3/class/Web3Eth/#sendTransaction)
- [setConfig](https://docs.web3js.org/api/web3/class/Web3Eth/#setConfig)
- [setMaxListenerWarningThreshold](https://docs.web3js.org/api/web3/class/Web3Eth/#setMaxListenerWarningThreshold)
- [setProvider](https://docs.web3js.org/api/web3/class/Web3Eth/#setProvider)
- [setRequestManagerMiddleware](https://docs.web3js.org/api/web3/class/Web3Eth/#setRequestManagerMiddleware)
- [sign](https://docs.web3js.org/api/web3/class/Web3Eth/#sign)
- [signTransaction](https://docs.web3js.org/api/web3/class/Web3Eth/#signTransaction)
- [signTypedData](https://docs.web3js.org/api/web3/class/Web3Eth/#signTypedData)
- [submitWork](https://docs.web3js.org/api/web3/class/Web3Eth/#submitWork)
- [subscribe](https://docs.web3js.org/api/web3/class/Web3Eth/#subscribe)
- [use](https://docs.web3js.org/api/web3/class/Web3Eth/#use)
- [fromContextObject](https://docs.web3js.org/api/web3/class/Web3Eth/#fromContextObject)







<br><br>
<br><br>
--  --  --  --  --  --  --  --  --  --  --  
<br><br>
<br><br>



### subscribe
- https://docs.web3js.org/api/web3/class/Web3Eth/#subscribe
```javascript
// You need provider which is able to use subscribe. You also must use WSS endpoint
const provider = 'wss://ethereum-mainnet.core.chainstack.com/xxxxxxxxxxx'
const web3 = new Web3(provider)

async function subscribeToNewBlocks() {
  const subscription = await web3.eth.subscribe('newBlockHeaders');
  subscription.on('data', handleNewBlock);
}

subscribeToNewBlocks()
```

<br><br>

#### Track swaps on Uniswap
```
// You need provider which is able to use subscribe. You also must use WSS endpoint
const provider = 'wss://ethereum-mainnet.core.chainstack.com/xxxxxxxxxxx'
const web3 = new Web3(provider)


const UNISWAP_ROUTER_ADDRESS = "0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D";
const SWAP_EXACT_ETH_FOR_TOKENS_SIGNATURE = "0x7ff36ab5";
const SWAP_EXACT_ETH_OR_TOKENS_SUPPORTING_FEE_ON_TRANSFER_TOKENS_SIGNATURE = "0xb6f9de95";

async function subscribeToNewBlocks() {
  const subscription = await web3.eth.subscribe('newBlockHeaders');
  subscription.on('data', handleNewBlock);
}

async function handleNewBlock(blockHeader) {
  console.log(`Got new block: ${blockHeader.number}`)
  const block = await web3.eth.getBlock(blockHeader.number, true)

  block.transactions.forEach((tx) => {
    if (
        tx.to && tx.to.toLowerCase() === UNISWAP_ROUTER_ADDRESS.toLowerCase() &&
        (
          tx.input.startsWith(SWAP_EXACT_ETH_FOR_TOKENS_SIGNATURE) || 
          tx.input.startsWith(SWAP_EXACT_ETH_OR_TOKENS_SUPPORTING_FEE_ON_TRANSFER_TOKENS_SIGNATURE)
        )
      ) {
      console.log("-----------------------------------------------------")
      console.log(`Incoming swap transaction: ${tx.hash}`);
      console.log(`From: ${tx.from}`);
      console.log(`Value: ${web3.utils.fromWei(tx.value, "ether")} ETH`);
      console.log("-----------------------------------------------------")
    }
  })
}

subscribeToNewBlocks()
```









<br><br>
<br><br>
<br><br>
<br><br>


### Balance

<br><br>

#### getBalance
```javascript
web3.eth.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1").then(console.log)
> 1000000000000n
```

##### Convert to USD
```javascript


// Funktion, um den aktuellen Ether-Preis von CoinMarketCap abzurufen
async function getCurrentEtherPrice() {
    const url = `https://pro-api.coinmarketcap.com/v1/cryptocurrency/quotes/latest?symbol=ETH&convert=USD`;

    try {
        const response = await axios.get(url, {
            headers: {
                'X-CMC_PRO_API_KEY': apiKey
            }
        });

        // Extrahieren Sie den aktuellen Preis von Ether in USD aus der API-Antwort
        const etherPriceUSD = response.data.data.ETH.quote.USD.price;
        return etherPriceUSD;
    } catch (error) {
        console.error('Fehler beim Abrufen des Ether-Preises:', error);
        return null;
    }
}

async function getBalanceInUSD(address) {
    try {
        const balanceWei = await web3.eth.getBalance(address);
        const etherPriceUSD = await getCurrentEtherPrice();
        const balanceEther = web3.utils.fromWei(balanceWei, 'ether');
        const balanceUSD = balanceEther * etherPriceUSD;
        return balanceUSD;
    } catch (error) {
        console.error('Fehler beim Berechnen der Balance in USD:', error);
        return null;
    }
}

const balanceUSD = await getBalanceInUSD(address)
console.log(`Balance in USD: ${balanceUSD}`)
```


















<br><br>
<br><br>
--  --  --  --  --  --  --  --  --  --  --  
<br><br>
<br><br>

### Block
- In der Blockchain-Technologie ist ein Block eine Datenstruktur, die Transaktionen zusammenfasst und speichert. Jeder Block enthält:

<br><br>

#### baseFeePerGas
- **Type:** BigInt
- **Description:** The minimum price per unit of gas that transactions in this block must pay to be included. Introduced with EIP-1559 to stabilize gas fees.

#### blobGasUsed
- **Type:** BigInt
- **Description:** Amount of gas used for blobs in this block. Blobs are data chunks attached to transactions.

#### difficulty
- **Type:** BigInt
- **Description:** A measure of how difficult it is to find a valid hash for this block. In Ethereum 2.0, this value is fixed at zero.

#### excessBlobGas
- **Type:** BigInt
- **Description:** Additional blob gas used above the expected level.

#### extraData
- **Type:** String
- **Description:** Arbitrary data that can be included by the miner, usually containing information about the mining pool.

#### gasLimit
- **Type:** BigInt
- **Description:** The maximum amount of gas that can be used in this block.

#### gasUsed
- **Type:** BigInt
- **Description:** The total amount of gas used by all transactions in this block.

#### hash
- **Type:** String
- **Description:** The unique identifier for this block, generated by hashing the block header.

#### logsBloom
- **Type:** String
- **Description:** A 256-byte bloom filter that includes logs for quick retrieval and filtering.

#### miner
- **Type:** String
- **Description:** The address of the account that mined this block.

#### mixHash
- **Type:** String
- **Description:** A hash used in the proof-of-work consensus algorithm.

#### nonce
- **Type:** BigInt
- **Description:** A value used in the proof-of-work consensus algorithm to generate the block hash.

#### number
- **Type:** BigInt
- **Description:** The block number, indicating its position in the blockchain.

#### parentBeaconBlockRoot
- **Type:** String
- **Description:** The root hash of the parent block in the beacon chain, part of Ethereum 2.0's consensus mechanism.

#### parentHash
- **Type:** String
- **Description:** The hash of the parent block, linking this block to the previous one in the chain.

#### receiptsRoot
- **Type:** String
- **Description:** The root hash of the receipts trie of the transactions included in this block.

#### sha3Uncles
- **Type:** String
- **Description:** The SHA-3 hash of the uncles list portion of this block.

#### size
- **Type:** BigInt
- **Description:** The size of this block in bytes.

#### stateRoot
- **Type:** String
- **Description:** The root hash of the state trie, representing the global state at this block.

#### timestamp
- **Type:** BigInt
- **Description:** The Unix timestamp for when this block was mined.

#### totalDifficulty
- **Type:** BigInt
- **Description:** The total difficulty of the chain until this block, used to determine the longest chain.

#### transactions
- **Type:** Array of Strings
- **Description:** A list of transaction hashes included in this block.



<br><br>
<br><br>

#### getBlock
- 
- https://docs.web3js.org/api/web3/class/Web3Eth/#getBlock
```javascript
let block = await web3.eth.getBlock('latest');
let number = block.number;
let transactions = block.transactions;
//console.log('Search Block: ' + transactions);
```










<br><br>
<br><br>
--  --  --  --  --  --  --  --  --  --  --  
<br><br>
<br><br>

### Transaction

#### getTransaction()
- https://docs.web3js.org/api/web3/class/Web3Eth/#getTransaction
```javascript
web3.eth.getTransaction('0x73aea70e969941f23f9d24103e91aa1f55c7964eb13daf1c9360c308a72686dc').then(console.log);
{
   hash: '0x73aea70e969941f23f9d24103e91aa1f55c7964eb13daf1c9360c308a72686dc',
   type: 0n,
   nonce: 0n,
   blockHash: '0x43202bd16b6bd54bea1b310736bd78bdbe93a64ad940f7586739d9eb25ad8d00',
   blockNumber: 1n,
   transactionIndex: 0n,
   from: '0x6e599da0bff7a6598ac1224e4985430bf16458a4',
   to: '0x6f1df96865d09d21e8f3f9a7fba3b17a11c7c53c',
   value: 1n,
   gas: 90000n,
   gasPrice: 2000000000n,
   input: '0x',
   v: 2709n,
   r: '0x8b336c290f6d7b2af3ccb2c02203a8356cc7d5b150ab19cce549d55636a3a78c',
   s: '0x5a83c6f816befc5cd4b0c997a347224a8aa002e5799c4b082a3ec726d0e9531d'
 }

web3.eth.getTransaction(
    web3.utils.hexToBytes("0x30755ed65396facf86c53e6217c52b4daebe72aa4941d89635409de4c9c7f9466d4e9aaec7977f05e923889b33c0d0dd27d7226b6e6f56ce737465c5cfd04be400"),
    { number: FMT_NUMBER.NUMBER , bytes: FMT_BYTES.HEX }
).then(console.log);
{
   hash: '0x73aea70e969941f23f9d24103e91aa1f55c7964eb13daf1c9360c308a72686dc',
   type: 0,
   nonce: 0,
   blockHash: '0x43202bd16b6bd54bea1b310736bd78bdbe93a64ad940f7586739d9eb25ad8d00',
   blockNumber: 1,
   transactionIndex: 0,
   from: '0x6e599da0bff7a6598ac1224e4985430bf16458a4',
   to: '0x6f1df96865d09d21e8f3f9a7fba3b17a11c7c53c',
   value: 1,
   gas: 90000,
   gasPrice: 2000000000,
   input: '0x',
   v: 2709,
   r: '0x8b336c290f6d7b2af3ccb2c02203a8356cc7d5b150ab19cce549d55636a3a78c',
   s: '0x5a83c6f816befc5cd4b0c997a347224a8aa002e5799c4b082a3ec726d0e9531d'
 }
```

#### get transactions from adress
- You can manually try to iterate over each block and then search for your address. E.g. you make -1n on the block and search in every block. But this does not make any send if you want to get any past transactions. But for present transactions it may be usefully. Transactions itself are not indexed doe
-  https://ethereum.stackexchange.com/questions/8900/how-to-get-transactions-by-account-using-web3-js

-  So recommended is to use etherscan.io or use something like this https://trueblocks.io with your own node
```javascript
class TransactionChecker {
    constructor(address) {
        this.address = address.toLowerCase();
        this.web3 = new Web3("https://mainnet.infura.io/v3/60968ff3b2f84a0ebdff7a993f4d080b");
}

async checkBlock() {
    let block = await this.web3.eth.getBlock('latest');
    let number = block.number;
    let transactions = block.transactions;
    //console.log('Search Block: ' + transactions);

    if (block != null && block.transactions != null) {
        for (let txHash of block.transactions) {
            let tx = await this.web3.eth.getTransaction(txHash);
            if (this.address == tx.to.toLowerCase()) {
                console.log("from: " + tx.from.toLowerCase() + " to: " + tx.to.toLowerCase() + " value: " + tx.value);
            }
        }
    }
  }
}

 var transactionChecker = new  TransactionChecker('0x69fb2a80542721682bfe8daa8fee847cddd1a267');
 transactionChecker.checkBlock();
```

</details>

























<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>


# web3-validator
- https://github.com/web3/web3.js/tree/4.x/packages/web3-validator
- https://docs.web3js.org/api/web3-validator
```javascript
import { validator } from 'web3-validator';

// To validate and throw
validator.validate(['uint8', 'string'], [val1, val2]);

// To validate and return error
const errors = validator.validate(['uint8', 'string'], [val1, val2], { silent: true });
```

<details><summary>Click to expand..</summary>

# Web3 Validator Functions

- [checkAddressCheckSum](https://docs.web3js.org/api/web3-validator/function/checkAddressCheckSum)
- [isAddress](https://docs.web3js.org/api/web3-validator/function/isAddress)
- [isBigInt](https://docs.web3js.org/api/web3-validator/function/isBigInt)
- [isBlockNumber](https://docs.web3js.org/api/web3-validator/function/isBlockNumber)
- [isBlockNumberOrTag](https://docs.web3js.org/api/web3-validator/function/isBlockNumberOrTag)
- [isBlockTag](https://docs.web3js.org/api/web3-validator/function/isBlockTag)
- [isBloom](https://docs.web3js.org/api/web3-validator/function/isBloom)
- [isBoolean](https://docs.web3js.org/api/web3-validator/function/isBoolean)
- [isBytes](https://docs.web3js.org/api/web3-validator/function/isBytes)
- [isContractAddressInBloom](https://docs.web3js.org/api/web3-validator/function/isContractAddressInBloom)
- [isFilterObject](https://docs.web3js.org/api/web3-validator/function/isFilterObject)
- [isHex](https://docs.web3js.org/api/web3-validator/function/isHex)
- [isHexPrefixed](https://docs.web3js.org/api/web3-validator/function/isHexPrefixed)
- [isHexStrict](https://docs.web3js.org/api/web3-validator/function/isHexStrict)
- [isHexString](https://docs.web3js.org/api/web3-validator/function/isHexString)
- [isHexString32Bytes](https://docs.web3js.org/api/web3-validator/function/isHexString32Bytes)
- [isHexString8Bytes](https://docs.web3js.org/api/web3-validator/function/isHexString8Bytes)
- [isInBloom](https://docs.web3js.org/api/web3-validator/function/isInBloom)
- [isInt](https://docs.web3js.org/api/web3-validator/function/isInt)
- [isNullish](https://docs.web3js.org/api/web3-validator/function/isNullish)
- [isNumber](https://docs.web3js.org/api/web3-validator/function/isNumber)
- [isObject](https://docs.web3js.org/api/web3-validator/function/isObject)
- [isString](https://docs.web3js.org/api/web3-validator/function/isString)
- [isTopic](https://docs.web3js.org/api/web3-validator/function/isTopic)
- [isTopicInBloom](https://docs.web3js.org/api/web3-validator/function/isTopicInBloom)
- [isUInt](https://docs.web3js.org/api/web3-validator/function/isUInt)
- [isUint8Array](https://docs.web3js.org/api/web3-validator/function/isUint8Array)
- [isUserEthereumAddressInBloom](https://docs.web3js.org/api/web3-validator/function/isUserEthereumAddressInBloom)
- [isValidEthBaseType](https://docs.web3js.org/api/web3-validator/function/isValidEthBaseType)
- [validateNoLeadingZeroes](https://docs.web3js.org/api/web3-validator/function/validateNoLeadingZeroes)

<br><br>
<br><br>

# isAddress
- https://docs.web3js.org/api/web3-validator/function/isAddress
```javascript
import { isAddress } from 'web3-validator';
const check = isAddress('0x1eF800xxxxxx'); // return false
console.log(check)
```

</details>



