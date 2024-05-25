# web3.js-cheat-sheet
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

<br><br>
<br><br>

## Create random wallet
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

## Send transactions
```javascript
//add an account to a wallet
const account = web3.eth.accounts.wallet.add('0x50d349f5cf627d44858d6fcb6fbf15d27457d35c58ba2d5cfeaf455f25db5bec');

//create transaction object to send 1 eth to '0xa32...c94' address from the account[0]
const tx = 
{ 
    from: account[0].address,
    to: '0xa3286628134bad128faeef82f44e99aa64085c94', 
    value: web3.utils.toWei('1', 'ether')
};
//the `from` address must match the one previously added with wallet.add

//send the transaction
const txReceipt = await web3.eth.sendTransaction(tx);

console.log('Tx hash:', txReceipt.transactionHash)
// ↳ Tx hash: 0x03c844b069646e08af1b6f31519a36e3e08452b198ef9f6ce0f0ccafd5e3ae0e
```



























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
