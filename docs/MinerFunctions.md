
### Tellor Deployed Addresses

Mainnet - [0x0ba45a8b5d5575935b8158a88c631e9f9c95a2e5](https://etherscan.io/address/0x0ba45a8b5d5575935b8158a88c631e9f9c95a2e5)

Rinkeby - [0xFe41Cb708CD98C5B20423433309E55b53F79134a](https://rinkeby.etherscan.io/address/0xFe41Cb708CD98C5B20423433309E55b53F79134a)

</br>

## Miner functions <a name="miner-fx"> </a>  
Miners engage in a POW competition to find a nonce which satisfies the requirement of the challenge.  The first five miners who solve the PoW puzzle provide the nonce, requestId, and value and receive native tokens in exchange for their work.  The oracle data submissions are stored in contract memory as an array - which is subsequently operated upon to derive the median value.

## depositStake()

This function allows miners to deposit their stake.

```javascript
depositStake() 
```
## requestStakingWithdraw()

This funciton allows the miner to initiate their stake withdraw. Once the request is initiated there is a 7 day waiting period before being able to withdraw. The miner must be in good standing(not under dispute) to be able to initiate this request.

```javascript
requestStakingWithdraw()
```

## withdrawStake()

This funtion allows miners to withdraw their stake once the 7 day waiting period has expired. The miner must be in good standing(not under dispute) to be able to withdraw. 

```javascript
withdrawStake()
```

## getCurrentVariables
Miners need to extract the current challenge, requestId and difficulty by calling the getCurrentVariables function before they can begin solving the PoW.

```javascript
getCurrentVariables()
```

The getCurrentVariables solidity function returns all the necessary variables. 
```solidity
    function getCurrentVariables(TellorStorageStruct storage self) internal view returns(bytes32, uint, uint,string memory,uint,uint){    
        return (self.currentChallenge,self.uintVars[keccak256("currentRequestId")],self.uintVars[keccak256("difficulty")],self.requestDetails[self.uintVars[keccak256("currentRequestId")]].queryString,self.requestDetails[self.uintVars[keccak256("currentRequestId")]].apiUintVars[keccak256("granularity")],self.requestDetails[self.uintVars[keccak256("currentRequestId")]].apiUintVars[keccak256("totalTip")]);
    }
```
## submitMiningSolution
Miners can use the submitMiningSolution function to submit the PoW, the top 5 requestId, and off-chain values for them. Production and test python miners are available under the miner subdirectory [here](https://github.com/tellor-io/TellorCore/tree/master/miner).  The PoW challenge is different than the regular PoW challenge used in Bitcoin. 

```javascript
    function submitMiningSolution(string _nonce, uint256`[5]` _requestId, uint256`[5]` _value)   
```
where 
  * nonce -- is the solution string submitted by miner
  * \_requestId -- is an array containing the 5 requestIds for the request on queue
  * value -- is an array contaning the 5 queried values correspoding to the requestIds