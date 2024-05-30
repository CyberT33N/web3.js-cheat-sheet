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




































<br><br>
<br><br>

______________________________________________________
______________________________________________________

<br><br>
<br><br>

# Acounts

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





































<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Contract

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
<br><br>


## Query past events
```javascript
//get past `Transfer` events from block 18850576
const eventTransfer = await uniswapToken.getPastEvents('Transfer', { fromBlock: 18850576 });

console.log(eventTransfer);
// ↳ [{...},{...}, ...] array with all the events emitted
//you can only query logs from the previous 100_000 blocks 
```





























<br><br>
<br><br>
_________________________________________
_________________________________________
<br><br>
<br><br>

# Events

<br><br>
<br><br>

## Listening to live events
- You MUST initialize the Web3 provider with a WebSocket endpoint to subscribe to live events
```javascript
import { Web3 } from 'web3';

//WebSocket provider
const web3 = new Web3('wss://ethereum.publicnode.com');

//instantiate contract
const uniswapToken = new web3.eth.Contract(abi, address)

//create the subcription to all the 'Transfer' events
const subscription = uniswapToken.events.Transfer();

//listen to the events
subscription.on('data',console.log);
// ↳ [{...},{...}, ...] live events will be printed in the console
```





































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


# Web3Eth
- The Web3Eth allows you to interact with an Ethereum blockchain.

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

### getBalance
```javascript
web3.eth.getBalance("0x407d73d8a49eeb85d32cf465507dd71d507100c1").then(console.log)
> 1000000000000n
```

#### Convert to USD
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

### Block
- In der Blockchain-Technologie ist ein Block eine Datenstruktur, die Transaktionen zusammenfasst und speichert. Jeder Block enthält:


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

