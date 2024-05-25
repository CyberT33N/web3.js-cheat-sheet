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
