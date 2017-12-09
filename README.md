# etherbox
Ethereum smart contract for ether savings using game theory. This project is being developed as a MVP for [Niterói's Startup Weekend](http://communities.techstars.com/brazil/niteroi/startup-weekend/11519)

Deployed at [0xfC324E3393e8b3915F453EB93661FfC1c045E7E3](https://rinkeby.etherscan.io/address/0xfC324E3393e8b3915F453EB93661FfC1c045E7E3)

### Development testnet

start rinkeby testnet with:

```
geth --networkid=4 --datadir=$HOME/.rinkeby --cache=512 --ethstats="yournode:Respect my authoritah!@stats.rinkeby.io" --bootnodes=enode://24ac7c5484ef4ed0c5eb2d36620ba4e4aa13b8c84684e1b4aab0cebea2ae45cb4d375b77eab56516d34bfbd3c1a833fc51296ff084b770b94fb9028c4d25ccf@52.169.42.101:30303 --rpc --rpcapi="personal,eth,network,db,web3"
```

or 

```
geth --rinkeby --syncmode "fast" --rpc --rpcapi="personal,eth,network,db,web3"
```

You can compile the smart contract at [ethfiddle](https://ethfiddle.com/) for testing porpouse.

To receive faucet ether, go to https://faucet.rinkeby.io/.

### How it works?

Each deposit to the Ether Box locks your ether and issues you with ETB tokens. These tokens are then used to make a withdrawal and burned once the withdrawal is processed.

A normal withdrawal would take ~30 days to process (actually there is a block counting), costing only the gas fee. The holder can withdraw their deposit earlier, but they must pay a %1 fee. The fees will be added to a fee-pot. Holders can claim a chunk from the fee-pot, with the following rules:

* Tokens will be issued when sending ETH to the contract: the holder receive tokens with the rate 1:1 ETB/ETH
* Normal ETH Withdrawal: must wait until after 30 days
* Immediate ETH withdrawal: Must pay a 1% fee
* For all Withdrawals: tokens will be destroyed after withdrawal. Ether to be sent back to the address that was holding the tokens.
* Tokens pending normal withdrawal will be locked until after 30 days.
* While tokens are locked, they cannot be transferred or sent.
* While tokens are locked, you cannot add more ETH to the address where the tokens are locked.
* When the wait period is over, you must complete the withdrawal manually. If the Fee Pot is not empty, you will automatically claim a reward from the Fee Pot
* When claiming the reward from the Fee Pot (Complete Withdrawal), the following formula is used:
   ```reward = feePot * v / totalSupply```
Where ```v``` is the amount you had when the withdrawal was requested, and ```totalSupply``` is the total amount of tokens in circulation at the time when Complete Withdrawal was called
* After withdrawal to ETH, tokens are burned, thus deflating the token supply.

### Game Theory
 
If enough holders send ETH to the contract, the possibility of some of them doing a quick withdrawing (paying the 1% fee) makes it happen more often. So, the fee pot would increase faster, generating more extra ether for every holder. The colective benefits for holding ether on the contract, as it is disadivantageous to quick withdraw the money.

### Future Improvements

* A small chunk of the smart contract funds (10%) can be used for day trading with a trusted partner, generating more revenue to holders.
* A chunk of the smart contract funds (30%) can be used for lending on behalf of a trusted partner, and the interest revenue would split among holders.
