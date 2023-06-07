## Router Protocol

<!-- <p align="center" >

<img src="https://user-images.githubusercontent.com/124175970/224509096-12e4864a-6819-4c8c-8998-41c7a96ba026.jpg" />
  </p> -->

<!-- ![router-protocol-crypto-ninjas](https://user-images.githubusercontent.com/124175970/224509096-12e4864a-6819-4c8c-8998-41c7a96ba026.jpg) -->

<img src="https://user-images.githubusercontent.com/124175970/224509096-12e4864a-6819-4c8c-8998-41c7a96ba026.jpg" width="8000000em" height="400em" />


# `Company Information`

Router Protocol is a solution introduced to address the issues hindering the usability of cross-chain liquidity migration in the DeFi ecosystem. It acts as a bridge connecting various layer 1 and layer 2 blockchains, allowing for the flow of contract-level data across them. The Router Protocol can either transfer tokens between chains or initiate operations on one chain and execute them on another.

# ⭐️ `Star us`

If this repository helps you build cross-chain dapps faster and easier - please star this project, every star makes us very happy!

# 🤝 `Need help?`

If you need help or have other some questions - don't hesitate to write in our discord channel and we will check asap. [Discord link](https://discord.gg/z7v3mrUE). The best thing about this is the super active community ready to help at any time! We help each other.

# 🤝 `Clone or fork this repository`

```sh
https://github.com/router-resources/RouterProtocol
```

## `Router Documentation`
To gain a deep and thorough understanding of the underlying concepts and functionalities, we highly recommend exploring the official documentation of Router Protocol. This comprehensive resource serves as an invaluable reference, providing detailed insights and explanations for every aspect of Router Protocol
[Official documentation of Router Protocol](https://docs.routerprotocol.com/)

## `Router Protocol Litepaper`
To gain a concise understanding of the core principles,problem solved, we invite you to explore the Litepaper of Router Protocol. This concise yet comprehensive document serves as a definitive guide, presenting a high-level overview of Router Protocol's vision, architecture, and ecosystem.

[![image](https://firebasestorage.googleapis.com/v0/b/tisperse.appspot.com/o/Screenshot%202023-06-07%20064310.png?alt=media&token=2bfd9728-b35b-4a12-aa78-b05fbe1efa3c&_gl=1*18pv93f*_ga*NjY1NTQ1MDAzLjE2ODYxMDExODQ.*_ga_CW55HF8NVT*MTY4NjEwMTE4NC4xLjEuMTY4NjEwMTI1My4wLjAuMA..)](https://drive.google.com/file/d/1g_JZUb9ArDdYckSgZsLTIZmQMSFc56IV/view?usp=sharing)
 


## `Understanding Router CrossTalk`

**Overview**

Router's CrossTalk library is a tool that makes it easy for different blockchains to communicate with each other. This library is designed to work with Router's infrastructure, and allows contracts on one blockchain to talk to contracts on another blockchain. You can use this library in your own development projects to make it easier for your contracts to communicate across different blockchains, without disrupting other parts of your project.

**Gateway Contracts**

Gateway contracts are contracts which are pre-deployed on supported blockchains for cross-chain communication.The source chain's gateway contract communicates with the destination chain's gateway contract, enabling communication between application contracts deployed on different chains

[Gateway Contract Addresses](https://lcd.testnet.routerchain.dev//router-protocol/router-chain/multichain/chain_config)

**CrossTalk Workflow**

![new-high-level-workflow-62293f72aac999e958af622280b3e15c](https://github.com/router-resources/ERC20-Cookbook/assets/124175970/de63b2db-e582-43fc-8b5a-cf27099b1ce5)


When a user wants to execute a cross-chain request, they call the "iSend" function on the Router's Gateway contract. They pass the payload of data to be transferred from the source chain to the destination chain along with the necessary parameters. The "iSend" function sends the data to the destination chain where the user's contract with the "iRecieve" function is waiting to receive it.Once the data is received, the "iRecieve" function processes it on the destination chain. After processing the data, the destination chain sends an acknowledgment back to the source chain where "iAck" function in the user's contract is used to handle it.

**Understanding CrossTalk Functions**

Router’s Gateway contracts have a function named iSend that facilitates the transmission of a cross-chain message. Whenever users want to execute a cross-chain request, they can call this function by passing the payload to be transferred from the source to the destination chain along with the required parameters.

In addition to calling the aforementioned function, CrossTalk users will also have to implement two functions on their contracts:

To handle a cross-chain request on the destination chain, users are required to include a iRecieve function on their destination chain contracts.
To process the acknowledgment of their requests on the source chain, user will have to implement a iAck function on their source chain contracts.

**iSend parameters**

1. **version**: Current version of Gateway contract which can be queried from the Gateway contract using the following function.
2. **routeAmount**: If one wants to transfer Route tokens along with the call, they will have to pass the amount of tokens to be transferred here.
3. **routeRecipient:** If one wants to transfer Route tokens along with the call, they will have to pass the address of recipient on the destination chain to which Route tokens will be minted on destination chain. 
4. **destChainId:** Chain ID of the destination chain in string format.
5. **requestMetadata:** Some static information for the request. This is created so that iDapps don’t have to encode it on-chain, they can just send it as a parameter to their iDapp depending on the destination chain Id passed by the user. 
6. **requestPacket:** This is bytes encoded string consisting of two parameters:

    a. **destContractAddress:** This is the address of the smart contract on the destination chain which will handle the payload                that you send from the source chain to the destination chain.
    b. **payload:** This is bytes containing the payload that you want to send to the destination chain. This can be anything depending          on your utility. In this case ,it is the recipient address on destination chain and the amount of tokens to be sent on the                destination chain 


**iRecieve**

This function is called by the Gateway contract on the destination chain, which is triggered when a cross-chain transfer request is sent to the destination chain. This function receives 3 parameters:-

1. **requestSender**: A bytes array that represents the address of the contract on the source chain that initiated the cross-chain transfer request.

2. **packet**: A bytes array that containing the payload we sent from the source chain.

3. **srcChainId**: A string that represents the ID of the source chain from which the cross-chain transfer request originated.

**iAck**

Leveraging the iAck function and its parameters allows us to effectively handle requests from the destination chain and send acknowlegment back to the source chain

Let's take a closer look at the parameters involved in the iAck function.

1. **requestIdentifier**: This parameter corresponds to the nonce received in the iSend() function on the source chain's Gateway contract. It serves as a unique identifier, allowing us to verify the request's execution on the destination chain. By matching the requestIdentifier, we can ensure the integrity of the cross-chain communication.

2. **execFlags**: The execFlags parameter is a boolean value that indicates the status of a read request. It provides information on whether the request was successful or encountered any errors during execution. By checking the execFlags, we can determine the outcome of the read request and handle any errors or exceptions accordingly.

3. **execData**: This parameter contains the result of all the read calls made in a read request, encoded in bytes. It carries the data obtained from executing the requested read operations on the destination chain. By accessing the execData, we can retrieve the desired information and utilize it for further processing or actions.

Cheatsheet

[Router CrossTalk CheatSheet](https://drive.google.com/file/d/1YFAfpgAV22DsYhqY6yYmlxkaVqB8PjuH/view?usp=sharing)



## `iDapps Tutorial`

**CrossChain ERC20**

## `What is an ERC20 Token ?`

![_117548721_nfts2](https://ph-files.imgix.net/a384cba0-ea83-4b99-b92b-06ab3639b493.gif?auto=format)

ERC20 Tokens are digital assets that are created using the Ethereum blockchain technology. They are programmable tokens that can be used to represent various types of assets, such as loyalty points, shares of stock, or even cryptocurrencies. ERC20 Tokens are fungible, which means that each token has the same value and can be exchanged for another token of the same value.

The "ERC20" part of the name refers to the technical standard that is used to create and manage these tokens. This standard defines the rules for creating new tokens and the functions that can be used to transfer and manage them.

## `How to make a simple ERC20 Token ?`

![ERC20](https://moralis.io/wp-content/uploads/2021/08/21_08_How-to-Create-Your-Own-ERC-20-Token-in-10-Minutes.jpg)

ERC20 is a standard for creating fungible tokens on the Ethereum blockchain. The ERC20 standard is like a set of rules that all tokens on the Ethereum blockchain must follow. Think of it like a recipe for making a soup - you need certain ingredients and instructions to make sure it turns out right.

ERC20 defines a specific set of functions that tokens must have, like the ability to be transferred from one address to another, the ability to check the token balance of an address, and the ability to approve another address to transfer tokens on behalf of the owner. These functions are like different steps in the recipe.

Some of the functions included in the ERC20 standard are:

**totalSupply:** This function returns the total number of tokens in existence.

**balanceOf:** This function returns the token balance of a specific address.

**transfer:** This function allows the owner of a token to transfer tokens to another address.

**approve:** This function allows an address to approve another address to transfer tokens on its behalf.

**allowance:** This function returns the amount of tokens that an approved address is allowed to transfer.

**transferFrom:** This function allows an approved address to transfer tokens on behalf of the owner.

Instead of writing all the code for creating an ERC20 token from scratch, developers can use OpenZeppelin's pre-written code to make their own tokens. This can save a lot of time and effort and can also help ensure that the code works correctly and is secure.

With OpenZeppelin, developers can just follow the pre-written code to create their own ERC20 tokens without having to start from scratch. It's kind of like using pre-made ingredients to cook a meal - instead of making each ingredient from scratch, you can just use pre-made ingredients to cook something faster and easier.

**Simple ERC20 contract using Openzeppelin**

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, ERC20Burnable, Ownable {
    constructor() ERC20("MyToken", "MTK") {}

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }
}
```



You can use the above code and deploy using [Remix IDE](https://www.remix.ethereum.org) and Hurray !, you've made your own crypto currency.


## `What is a CrossChain ERC20 Token ?`

CrossChain ERC20 tokens are tokens that can exist and be traded on multiple different blockchain networks. This means that an ERC20 token created on one blockchain network, such as Ethereum, can be moved to and traded on another blockchain network, such as Binance Smart Chain or Polygon.

Imagine you have an ERC20 token on the Ethereum blockchain that represents a digital asset. If you want to sell or trade that token on another blockchain network, you would need to create a new token on that network, which can be time-consuming and costly. However, with CrossChain ERC20 tokens, you can simply transfer the original token to the new blockchain network, enabling you to sell or trade it without having to go through the process of creating a new one.

<img width="461" alt="image" src="https://stealthex.io/blog/wp-content/uploads/2022/06/%D0%A1ross_chain_bridge-5-min.png">


# `CrossChain ERC-20`

> Effortlessly transfer ERC-20 tokens from one chain to another. Made using Router Cross-Talk.

This project is built with [Router CrossTalk](https://dev.routerprotocol.com/crosstalk-library/overview/introduction)


## `Initiating the Contract`

For initiating the smart contract named "CrossChainERC20", the contract imports four external contracts :-

1. **IDapp.sol**

2. **IGateway.sol**

3. **Utils.sol**

4. **ERC20.sol**

The "IDapp.sol" and "IGateway.sol" contracts are imported from the "evm-gateway-contract/contracts" and "ERC20.sol" from "openzeppelin/contracts/token".The "CrossChainERC20" contract implements the "IDapp" and "ERC20.sol" contract by inheriting from them. This means that the "CrossChainERC20" contract must have the functions and variables defined in the "IDapp" contract. By importing and implementing these contracts, the "CrossChainERC20" contract will have access to their functionality .

```sh
//SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.8.0 <0.9.0;

import "@routerprotocol/evm-gateway-contracts/contracts/IDapp.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/IGateway.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/Utils.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

contract CrossChainERC20 is ERC20, IDapp {
}
```
## `Creating State Variables and the Constructor`

The smart contract has the following state variables:

1. **onwer** - an address variable which stores the address from which the contract has been deployed.

2. **gatewayContract** - an address variable which holds the address of the gateway contract. Gateway contracts are contracts which are pre-deployed on supported blockchains for cross-chain communication.The source chain's gateway contract communicates with the destination chain's gateway contract, enabling communication between application contracts deployed on different chains. Find GatewayContract Addresses [here](https://lcd.testnet.routerchain.dev//router-protocol/router-chain/multichain/chain_config)

3. **destGasLimit** - a uint64 variable which indicates the amount of gas required to execute the function that will handle cross-chain requests on the destination chain.

4. **ourContractOnChains** - a mapping which maps a chain type and chain ID to the address of CrossChainERC20 contract deployed on different chains. This mapping will be used to set the address of the destination contract to the source contract and visa versa

The constructor of the smart contract takes three parameters:

1. **gatewayAddress** - an address variable which holds the address of the gateway contract.

2. **feePayerAddress** - a string variable which holds the address on the Router Chain from which the fees is deducted.

Inside ERC20 contract contructor, pass the name of your token followed by its symbol.

The smart contract extends the ERC20 standard and includes all the required functions and variables such as balanceOf, totalSupply, mint, burn and others.

```sh
address public owner;

  IGateway public gatewayContract;

  uint64 public _destGasLimit;

  mapping(string => bytes) public ourContractOnChains;

  constructor(
    address payable gatewayAddress,
    string memory feePayerAddress
  ) ERC20("My Token", "MTK") {
    gatewayContract = IGateway(gatewayAddress);
    owner = msg.sender;
    gatewayContract.setDappMetadata(feePayerAddress);
  }

```

## `Setting up the Destination Contract on the Source Contract`

**setContractOnChain**:-

The given code defines a setter function setContractOnChain which allows the owner to set the address of a destination contract on the source chain and visa versa. The function takes 2 parameters:

chainId - a string which represents the ID of the chain where the contract is deployed.
contractAddress - an address variable which holds the address of the CrossChainERC20 contract deployed on the other chain we want to send the tokens to.
The function first checks whether the caller is the owner by comparing the msg.sender with the admin address variable. If the caller is not the owner, the function will revert with an error message "only owner".

If the caller is the owner, the function will convert the contractAddress to bytes using the toBytes function and store it in the ourContractOnChains mapping using the chainType and chainId as the keys.

```sh
 function setContractOnChain(
    string memory chainId,
    address contractAddress
  ) external {
    require(msg.sender == owner, "only owner");
    ourContractOnChains[chainId] = toBytes(contractAddress);
  }
```
## `Transferring tokens from a source chain to a destination chain`

**transferCrossChain:-**

This function allows a user to transfer their tokens from their account on source chain to their account on a destination chain. The function burns the specified amount of tokens from the user's account, creates a cross-chain communication request to the destination chain, and passes the transfer parameters as payload in the request.

The function accepts the following parameters:

1. **amount**: A uint256 variable specifying the amount of tokens we want to transfer to the recipient on the destination chain

2. **_dstchainId**: A string representing the ID of the destination chain.

3. **recipient**: Wallet address on the destination chain we want to transfer our tokens to.

5. **requestMetadata**: Some static information for the request. This is created so that iDapps don’t have to encode it on-chain, they can just send it as a parameter to their iDapp depending on the destination chain Id passed by the user. The request metadata is a bytes encoded string consisting of the following parameters: 
```sh
    uint64 destGasLimit;
    uint64 destGasPrice;
    uint64 ackGasLimit;
    uint64 ackGasPrice;
    uint128 relayerFees;
    uint8 ackType;
    bool isReadCall;
    string asmAddress;
```
The function burns the amount of tokens specified in the variable "amount" from the user's account and creates a cross-chain communication request to the destination chain. 

The function calls the iSend Function of the Gateway Contract to generate the cross-chain communication request to the destination chain.The iSend Function function is used to send a request to the Destination Chain .To execute a cross-chain request, users can call this function and pass the payload and required parameters from the source to the destination chain.

**iSend Function** function takes in these parameters:-

1. **version**: Current version of Gateway contract which can be queried from the Gateway contract using the following function.
2. **routeAmount**: If one wants to transfer Route tokens along with the call, they will have to pass the amount of tokens to be transferred here.
3. **routeRecipient:** If one wants to transfer Route tokens along with the call, they will have to pass the address of recipient on the destination chain to which Route tokens will be minted on destination chain. 
4. **destChainId:** Chain ID of the destination chain in string format.
5. **requestMetadata:** Some static information for the request. This is created so that iDapps don’t have to encode it on-chain, they can just send it as a parameter to their iDapp depending on the destination chain Id passed by the user. 
6. **requestPacket:** This is bytes encoded string consisting of two parameters:

    a. **destContractAddress:** This is the address of the smart contract on the destination chain which will handle the payload                that you send from the source chain to the destination chain.
    b. **payload:** This is bytes containing the payload that you want to send to the destination chain. This can be anything depending          on your utility. In this case ,it is the recipient address on destination chain and the amount of tokens to be sent on the                destination chain 
    
```sh
function transferCrossChain(
    uint256 amount,
    string calldata destChainId,
    string calldata recipient,
    bytes calldata requestMetadata
  ) public payable {
    require(
      keccak256(ourContractOnChains[destChainId]) !=
        keccak256(toBytes(address(0))),
      "contract on dest not set"
    );

    require(
      balanceOf(msg.sender) >= amount,
      "ERC20: Amount cannot be greater than the balance"
    );

   
    _burn(msg.sender, amount);

    // encoding the data that we need to use on destination chain to mint the tokens there.
    bytes memory packet = abi.encode(recipient, amount);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );

    gatewayContract.iSend{ value: msg.value }(
      1,
      0,
      string(""),
      destChainId,
      requestMetadata,
      requestPacket
    );
  }
  ```
 
## `Handling a cross-chain request`

**iReceive:-**

This function is called by the Gateway contract on the destination chain, which is triggered when a cross-chain transfer request is sent to the destination chain. This function receives 3 parameters:-

1. **requestSender**: A bytes array that represents the address of the contract on the source chain that initiated the cross-chain transfer request.

2. **packet**: A bytes array that containing the payload we sent from the source chain.

3. **srcChainId**: A string that represents the ID of the source chain from which the cross-chain transfer request originated.

The function first checks that the call is made only by the Gateway contract and that the request is received from our contract on the source chain. If the conditions are not met, the function will revert the transaction.

The packet that was sent with the cross-chain transfer request contains recipient's address, the amount of tokens to be minted on the destination chain. The function decodes the packet using abi.decode() function.

After decoding the packet, the function uses the _mint function of the ERC-20 contract from the OpenZeppelin library to mint the ERC-20 tokens to the recipient's address on the destination chain. 

Finally, the function returns an empty string. Note : We have to return atleast an empty string as per the function definition.

```sh
function iReceive(
    string memory requestSender,
    bytes memory packet,
    string memory srcChainId
  ) external override returns (bytes memory) {
    require(msg.sender == address(gatewayContract), "only gateway");
    require(
      keccak256(ourContractOnChains[srcChainId]) ==
        keccak256(bytes(requestSender))
    );

    (bytes memory recipient, uint256 amount) = abi.decode(
      packet,
      (bytes, uint256)
    );
    _mint(toAddress(recipient), amount);

    return abi.encode(srcChainId);
  }
```
## `Getting metadata for the request`

Some static information for the request. This is created so that iDapps don’t have to encode it on-chain, they can just send it as a parameter to their iDapp depending on the destination chain Id passed by the user. The request metadata is a bytes encoded string consisting of the following parameters: 

1. **destGasLimit:** Gas limit required for execution of the request on the destination chain. This can be calculated using tools like [hardhat-gas-reporter](https://www.npmjs.com/package/hardhat-gas-reporter).

2. **destGasPrice:** Gas price of the destination chain. This can be calculated using the RPC of destination chain.
If you don’t want to calculate it, just send `0` in its place and Router Chain will handle the real time gas price for you. 

3. **ackGasLimit:** Gas limit required for execution of the acknowledgement coming from the destination chain back on the source chain. This can be calculated using tools like [hardhat-gas-reporter](https://www.npmjs.com/package/hardhat-gas-reporter).

4. **ackGasPrice:** Gas price of the destination chain. This can be calculated using the RPC of source chain as shown in the above [snippet](https://www.notion.so/EVM-to-Other-Chain-Flow-de922b13e0fa4d7b8c3c24590ff8ef65).

5. **relayerFees:** This is similar to priority fees that one pays on other chains. Router chain relayers execute your requests on the destination chain. So if you want your request to be picked up by relayer faster, this should be set to a higher number. If you pass really low amount, the Router chain will adjust it to some minimum amount.

6. **ackType:** When the contract calls have been executed on the destination chain, the iDapp has the option to get an acknowledgement back to the source chain.
    
We provide the option to the user to be able to get this acknowledgement from the router chain to the source chain and perform some operation based on it.
    
  1. **ackType = 0:** You don’t want the acknowledgement to be forwarded back to the source chain.
  2. **ackType = 1:** You only want to receive the acknowledgement back to the source chain in case the calls executed successfully on the destination chain and perform some operation after that.
  3. **ackType = 2:** You only want to receive the acknowledgement back to the source chain in case the calls errored on the destination chain and perform some operation after that.
  4. **ackType = 3:** You only want to receive the acknowledgement back to the source chain in both the cases (success and error) and perform some operation after that.
  
7. **isReadCall:** We provide you the option to query a contract from another chain and get the data back on the source chain through acknowledgement. If you just want to query a contract on destination chain, set this to `true`.

8. **asmAddress:** We also provide modular security framework for creating an additional layer of security on top of the security provided by Router Chain. These will be in the form of smart contracts on destination chain. The address of this contract needs to be passed in the form of string in this variable.
    
    The request metadata can be constructed using the following code:
    
```sh
function getRequestMetadata(
    uint64 destGasLimit,
    uint64 destGasPrice,
    uint64 ackGasLimit,
    uint64 ackGasPrice,
    uint128 relayerFees,
    uint8 ackType,
    bool isReadCall,
    string calldata asmAddress
  ) public pure returns (bytes memory) {
    bytes memory requestMetadata = abi.encodePacked(
      destGasLimit,
      destGasPrice,
      ackGasLimit,
      ackGasPrice,
      relayerFees,
      ackType,
      isReadCall,
      asmAddress
    );
    return requestMetadata;
  }
```
## `Handling the acknowledgement received from destination chain`

**iAck:-**

The iAck function is a public function that needs to be implemented in the contract to satisfy the IDapp interface.
The function takes three parameters: **eventIdentifier**, **execFlags**, and **execData** . However, since we are only implementing an empty function, we do not need to use these parameters.

The iAck function does not perform any operation here. It is implemented as an empty function and only serves as a placeholder to satisfy the interface requirements.

Therefore, the iAck function should be implemented with an empty body as shown in the code provided.

```sh
function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}

```

## `Full Code`

```sh
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.8.0 <0.9.0;

import "@routerprotocol/evm-gateway-contracts/contracts/IDapp.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/IGateway.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/Utils.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";

/// @title XERC20
/// @author Yashika Goyal
/// @notice This is a cross-chain ERC-20 smart contract to demonstrate how one can
/// utilise Router CrossTalk for making cross-chain tokens
contract XERC20 is ERC20, IDapp {
  // address of the owner
  address public owner;

  // address of the gateway contract
  IGateway public gatewayContract;

  // gas limit required to handle cross-chain request on the destination chain
  uint64 public _destGasLimit;

  // chain id => address of our contract in bytes
  mapping(string => bytes) public ourContractOnChains;

  constructor(
    address payable gatewayAddress,
    string memory feePayerAddress
  ) ERC20("My Token", "MTK") {
    gatewayContract = IGateway(gatewayAddress);
    owner = msg.sender;

    //minting 20 tokens to deployer initially for testing
    _mint(msg.sender, 20);

    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the fee payer address on Router Chain.
  /// @param feePayerAddress address of the fee payer on Router Chain.
  function setDappMetadata(string memory feePayerAddress) external {
    require(msg.sender == owner, "only owner");
    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the Router Gateway Contract.
  /// @param gateway address of the gateway contract.
  function setGateway(address gateway) external {
    require(msg.sender == owner, "only owner");
    gatewayContract = IGateway(gateway);
  }

  function mint(address account, uint256 amount) external {
    require(msg.sender == owner, "only owner");
    _mint(account, amount);
  }

  /// @notice function to set the address of our ERC20 contracts on different chains.
  /// This will help in access control when a cross-chain request is received.
  /// @param chainId chain Id of the destination chain in string.
  /// @param contractAddress address of the ERC20 contract on the destination chain.
  function setContractOnChain(
    string memory chainId,
    address contractAddress
  ) external {
    require(msg.sender == owner, "only owner");
    ourContractOnChains[chainId] = toBytes(contractAddress);
  }

  /// @notice function to generate a cross-chain token transfer request.
  /// @param destChainId chain ID of the destination chain in string.
  /// @param recipient address of the recipient of tokens on destination chain
  /// @param amount amount of tokens to be transferred cross-chain
  /// @param requestMetadata abi-encoded metadata according to source and destination chains
  function transferCrossChain(
    uint256 amount,
    string calldata destChainId,
    string calldata recipient,
    bytes calldata requestMetadata
  ) public payable {
    require(
      keccak256(ourContractOnChains[destChainId]) !=
        keccak256(toBytes(address(0))),
      "contract on dest not set"
    );

    require(
      balanceOf(msg.sender) >= amount,
      "ERC20: Amount cannot be greater than the balance"
    );

    // burning the tokens from the address of the user calling this function
    _burn(msg.sender, amount);

    // encoding the data that we need to use on destination chain to mint the tokens there.
    bytes memory packet = abi.encode(recipient, amount);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );

    gatewayContract.iSend{ value: msg.value }(
      1,
      0,
      string(""),
      destChainId,
      requestMetadata,
      requestPacket
    );
  }

  /// @notice function to get the request metadata to be used while initiating cross-chain request
  /// @return requestMetadata abi-encoded metadata according to source and destination chains
  function getRequestMetadata(
    uint64 destGasLimit,
    uint64 destGasPrice,
    uint64 ackGasLimit,
    uint64 ackGasPrice,
    uint128 relayerFees,
    uint8 ackType,
    bool isReadCall,
    string calldata asmAddress
  ) public pure returns (bytes memory) {
    bytes memory requestMetadata = abi.encodePacked(
      destGasLimit,
      destGasPrice,
      ackGasLimit,
      ackGasPrice,
      relayerFees,
      ackType,
      isReadCall,
      asmAddress
    );
    return requestMetadata;
  }

  /// @notice function to handle the cross-chain request received from some other chain.
  /// @param requestSender address of the contract on source chain that initiated the request.
  /// @param packet the payload sent by the source chain contract when the request was created.
  /// @param srcChainId chain ID of the source chain in string.
  function iReceive(
    string memory requestSender,
    bytes memory packet,
    string memory srcChainId
  ) external override returns (bytes memory) {
    require(msg.sender == address(gatewayContract), "only gateway");
    require(
      keccak256(ourContractOnChains[srcChainId]) ==
        keccak256(bytes(requestSender))
    );

    (bytes memory recipient, uint256 amount) = abi.decode(
      packet,
      (bytes, uint256)
    );
    _mint(toAddress(recipient), amount);

    return abi.encode(srcChainId);
  }

  /// @notice function to handle the acknowledgement received from the destination chain
  /// back on the source chain.
  /// @param requestIdentifier event nonce which is received when we create a cross-chain request
  /// We can use it to keep a mapping of which nonces have been executed and which did not.
  /// @param execFlag a boolean value suggesting whether the call was successfully
  /// executed on the destination chain.
  /// @param execData returning the data returned from the handleRequestFromSource
  /// function of the destination chain.
  function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}

  /// @notice function to convert type address into type bytes.
  /// @param a address to be converted
  /// @return b bytes pertaining to the address
  function toBytes(address a) public pure returns (bytes memory b) {
    assembly {
      let m := mload(0x40)
      a := and(a, 0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF)
      mstore(add(m, 20), xor(0x140000000000000000000000000000000000000000, a))
      mstore(0x40, add(m, 52))
      b := m
    }
  }

  /// @notice Function to convert bytes to address
  /// @param _bytes bytes to be converted
  /// @return addr address pertaining to the bytes
  function toAddress(bytes memory _bytes) internal pure returns (address addr) {
    bytes20 srcTokenAddress;
    assembly {
      srcTokenAddress := mload(add(_bytes, 0x20))
    }
    addr = address(srcTokenAddress);
  }
}
```
# 🎯 `Steps`


✍️ **Setting up your editor:**

Browse to [Remix IDE](https://remix.ethereum.org/) and create a new file with ".sol" extension.

💿 **Install all dependencies:**

You don't need to install any dependencies. Remix automatically downloads all the dependencies for you during the time of compile.

🧑‍💻 **Create your CrossChain ERC-20 token Contract:**

To create the contract for your CrossChain ERC-20 token , copy-paste the [`Code`](#Full-Code) in the Remix Work Area and compile it.
The Code has been comprehensively explained in this repository. Click [`here`](#Initiating-the-Contract) for the explanation.

🚀 **Deploying the Contract:**

You need to deploy the same contract on the source chain as well as the destination chain and pass in the required parameters to the [`constructer`](#Creating-state-variables-and-the-constructor) while deploying.

🔨 **Mint created ERC-20 token on Source Chain:**

In order to mint the created ERC-20 token on the source , mint function defined in openzeppelin can be used.

🤝 **Set destination contract to source contract and source contract to destination contract:**

To set destination contract to source contract and source contract to destination contract, we make use of setContractOnChain function. For more info, go to [`Setting up the Destination Contract on the Source Contract`](#Setting-up-the-Destination-Contract-on-the-Source-Contract)

💵 **Get Route Test Tokens on your wallet address :**

To get Route tokens on wallet address, copy the source contract address, visit https://devnet-faucet.routerprotocol.com/ , paste the address there and click on Get Route

🚂 **Transfer minted ERC-20 tokens from source chain to destination chain:**

To transfer minted ERC-20 tokens from source chain to destination chain, we make use of transferCrosschain function, which burns specified amount of tokens on source chain and mint the same amount on the destination chain. For more info, go to [`Transferring tokens from a source chain to a destination chain`](#Transferring-tokens-from-a-source-chain-to-a-destination-chain)

🔍 **Browse to [Router Testnet Explorer](https://explorer.testnet.routerchain.dev/crosstalks)** to see the transactions made. Wait for sometime till you see 4 green checks in your transaction column.This indicates, the tokens have been successfully transferred to the destination chain

📖 For more detailed steps , refer [Step by Step guide for CrossChain ERC-20](https://github.com/router-resources/Workshop-ERC20)


# `CrossChain ERC-721`

## `What is an ERC721 Token ?`

![_117548721_nfts2](https://user-images.githubusercontent.com/124175970/224860849-f037eb49-0288-49b8-a922-e08015b5280a.jpg)

ERC721 Tokens are digital assets that are created using the Ethereum blockchain technology. They are programmable tokens that can be used to represent various types of assets, such as loyalty points, shares of stock, or even cryptocurrencies. ERC20 Tokens are fungible, which means that each token has the same value and can be exchanged for another token of the same value.

The "ERC721" part of the name refers to the technical standard that is used to create and manage these tokens. This standard defines the rules for creating new tokens and the functions that can be used to transfer and manage them.

Here's an example of **Bored Ape** NFT series :-

![bored-ape-yacht-club-nft (1)](https://user-images.githubusercontent.com/124175970/224862305-ca762b98-a29c-4d6f-9693-9642f7c63ae1.gif)


## `How to make a simple NFT ?`

**ERC721 Standard**

![1_-Aq82tW0D4c9Xn8zHRYecw](https://user-images.githubusercontent.com/124175970/224863210-5b6262b9-e793-49ee-b1f0-1158eda92cf0.png)

ERC721 is a standard for creating non-fungible tokens on the Ethereum blockchain. The ERC721 standard is like a set of rules that all NFTs on the Ethereum blockchain must follow. Think of it like a recipe for making a cake - you need certain ingredients and instructions to make sure it turns out right.

ERC721 defines a specific set of functions that NFTs must have, like the ability to be transferred from one owner to another, the ability to check who owns a particular NFT, and the ability to create new NFTs. These functions are like different steps in the recipe.

**ERC721 Functions**

Some of the functions included in the ERC721 standard are:

**mint**: This function creates a new token and assigns it to an owner.

**transfer**: This function allows the owner of a token to transfer ownership to another address.

**balanceOf**: This function returns the number of tokens owned by a specific address.

**ownerOf**: This function returns the address of the current owner of a specific token.

**approve**: This function allows an address to approve another address to transfer ownership of a specific token.

**safeTransferFrom**: This function transfers ownership of a token from one address to another, but also includes additional safety checks to ensure the transfer is successful.

**What is OpenZeppelin ?**

Instead of writing all the code for creating an NFT from scratch, developers can use OpenZeppelin's pre-written code to make their own NFTs. This can save a lot of time and effort, and can also help ensure that the code works correctly and is secure.

It's kind of like using Lego blocks to build something - instead of making each block from scratch, you can just use pre-made blocks to build something faster and easier.

With OpenZeppelin, developers can just follow the pre-written code to create their own NFTs without having to start from scratch.

**Simple ERC721 contract using Openzeppelin**

```sh
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721Burnable.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/utils/Counters.sol";

contract MyToken is ERC721, ERC721Burnable, Ownable {
    using Counters for Counters.Counter;

    Counters.Counter private _tokenIdCounter;

    constructor() ERC721("MyToken", "MTK") {}

    function _baseURI() internal pure override returns (string memory) {
        return "<paste the url here>";
    }

    function safeMint(address to) public onlyOwner {
        uint256 tokenId = _tokenIdCounter.current();
        _tokenIdCounter.increment();
        _safeMint(to, tokenId);
    }
}
```

The contract is making use of the OpenZeppelin library which includes pre-written functions that make it easier to create NFTs.

Then the name and symbol of the NFT is given as parameters to the constructor.It's using three different pre-written contracts from OpenZeppelin:

**ERC721**: This is the main contract for creating NFTs. It defines the basic functions that NFTs should have, like the ability to be owned and transferred.

**ERC721Burnable** : This is an extension of ERC721 that allows the owner of an NFT to "burn" it, or destroy it completely. This can be useful for controlling the supply of an NFT.

**Ownable** : This is another extension of ERC721 that defines an "owner" for the NFT contract. Only the owner can perform certain functions, like creating new tokens.

The code also includes a "using" statement, which tells the contract to use a library called "Counters". This library includes a function called "Counter" that's used to keep track of the number of tokens that have been created.

The constructor function is defining the name and abbreviation of the NFT. The name is "MyToken" and the abbreviation is "MTK".

The "_baseURI" function is defining the URL where people can go to find more information about the NFT (metadata). This could be either IPFS or other decentralised storage networks. For this, you can make use of tools like [NFTUP](https://nft.storage/docs/how-to/nftup/)

The "safeMint" function is used to create new tokens and assign them to a specific owner. The owner of the NFT contract is the only one who can use this function. It uses the "Counter" function from the Counters library to keep track of the number of tokens that have been created. When a new token is created, the owner can assign it to a specific address.

You can use the above code and deploy using [Remix IDE](https://www.remix.ethereum.org) and Hurray !, you've made your own NFT .



## `What is a CrossChain NFT ?`

CrossChain NFTs are non-fungible tokens that can exist and be traded on multiple different blockchain networks. This means that an NFT created on one blockchain network, such as Ethereum, can be moved to and traded on another blockchain network, such as Binance Smart Chain or Polygon.

Imagine you have an NFT on the Ethereum blockchain that represents a piece of artwork. If you want to sell or trade that NFT on another blockchain network, you would need to create a new NFT on that network, which can be time-consuming and costly. However, with CrossChain NFTs, you can simply transfer the original NFT to the new blockchain network, enabling you to sell or trade it without having to go through the process of creating a new one.

<img width="461" alt="image" src="https://user-images.githubusercontent.com/124175970/224872315-6b455b3f-e822-400d-ab2d-cb81ad135f5d.png">


Router's crosstalk library: Provides functions that help to create cross-chain communication between different blockchain networks.



// SPDX-License-Identifier: UNLICENSED

From Solidity version ^0.6.8 SPDX license is introduced. You need to use SPDX-License-Identifier: <SPDX-License> in the first line of your contract with comments like shown above. Trust in smart contracts can be better established if their source code is available. Since making source code available always touches on legal problems with regards to copyright, the Solidity compiler encourages the use of machine-readable SPDX license identifiers. You can find all SPDX license lists here.

```sh 
pragma solidity >=0.8.0 <0.9.0;
```
Pragma is generally the first line of code within any Solidity file. pragma is a directive that specifies the compiler version to be used for current Solidity file. With the help of the pragma directive, you can choose the compiler version and target your code accordingly, as shown in the snippet above


```sh
import "@routerprotocol/evm-gateway-contracts/contracts/IDapp.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/IGateway.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/Utils.sol";
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
```

By importing, we mean combining two solidity files which means that the two files just share their interface. You can call external and public functions of the imported contract but not the internal traits which is the case with inheritance.

```
import the ERC721.sol, ERC721URIStorage.sol and Ownable.sol contracts from openzeppelin/contracts

Import the IDapp.sol,Utils.sol and  IGateway.sol from evm-gateway-contract/contracts. 

// This is how we inherit any contract into our contract using `is` identifier.
contract XERC721 is ERC721, IDapp{
```

By Inheriting, we can access all the non-private members including state variables and internal methods of inherited contract. 

Inherit the ERC721.sol contract into your main contract (XERC721). 

Inherit the IDapp contract into your main contract (XERC721).


```sh
constructor(
    address payable gatewayAddress,
    string memory feePayerAddress
  ) ERC721("My Token", "MTK") {
    gatewayContract = IGateway(gatewayAddress)
    gatewayContract.setDappMetadata(feePayerAddress);
    owner = msg.sender;

  }
```


When we inherit a contract (which has a constructor) into our contract, we have to initiate its constructor like I have shown above. For instance, we had to inherit ERC721.sol. This file has its constructor that needs the name and symbol of the NFT. So we now need to provide that name and symbol through the constructor that we define in our contract because our contract is nothing but a logic written on the top of ERC721.sol code.

The constructor of the smart contract takes 2 parameters:
gatewayAddress - an address variable which holds the address of the gateway contract.
feePayerAddress - a string variable which holds the address on the Router Chain from which the fees is deducted.
Further, we pass gatewayAddress to IGateway function which sets the gateway address and call setDappMetadata function of gatewayContract which sets the feePayerAddress.
Finally, we set the owner to be the person who deployed the contract by the following command:-
owner = msg.sender;

```sh

function iReceive(
    string memory requestSender,
    bytes memory packet,
    string memory srcChainId
  ) external override returns (bytes memory) {}

function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}
				
				```

So, when we use the interface contracts into our contract, we are also liable to implement the above two functions mandatorily, otherwise our contract would not compile successfully. For now, I have kept the function implementations empty {}. We will add the logic into them according to our requirements as we move ahead with the project.


It is important to ensure that any code snippets for state variables are placed within the same smart contract that was imported and inherited in the previous step. This is because state variables need to be defined within a contract, and the inherited contracts and imported files define the structure of the contract that is being created.

State variables are variables that hold values that define the current state of the contract. They are declared at the contract level, outside any function, and are used to store data that can be accessed by multiple functions within the contract. Examples of state variables in an NFT contract may include the name of the NFT, the owner of the NFT, and the current supply of the NFT.

A constructor, on the other hand, is a special function that is called when the contract is deployed. It is used to initialize the state variables of the contract and set their initial values. The constructor function is defined with the same name as the contract, and it can take arguments that set the initial values of the state variables

Make sure that you continue putting the code snippets into the same smart contract that we started in the Import&Inherit step.



These are the different state variables that we have used in our contract. Each line has been explained, what it actually does.
address public admin;
Admin is the address that is to be used for access control purposes.




IGateway public gatewayContract;
gatewayContract is the address of the contract which will be used to interact with the Router Chain.




mapping(string => string) public ourContractOnChains;   
ourContractOnChains is a mapping that stores the addresses of the counterparts of our NFT contract on different chains. The chain Id is mapped to the contract address on that chain. Chain Id is the ID of the chain in string format.You can find them here https://lcd.testnet.routerchain.dev//router-protocol/router-chain/multichain/chain_config




```sh
	struct TransferParams {
   	 uint256 nftId;
   	 bytes recipient;
    	string uri;
  	}
```

TransferParams is a struct that holds the Id of the NFT, wallet address that will hold the NFT and the URI for the NFT.




Some static information is required for the request so that dapps don’t have to encode it on-chain, they can just send it as a parameter to their dapp depending on the destination chain Id passed by the user. The request metadata is a bytes encoded string consisting of the following parameters:
destGasLimit: Gas limit required for execution of the request on the destination chain. This can be calculated using tools like hardhat-gas-reporter. Here, in this case will are passing 500000 as the destGasLimit
destGasPrice: Gas price of the destination chain. This can be calculated using the RPC of destination chain. If you don’t want to calculate it, just send 0 in its place and Router Chain will handle the real time gas price for you. In this case, we will be passing 30000000000 as destGasPrice.
ackGasLimit: Gas limit required for execution of the acknowledgement coming from the destination chain back on the source chain. This can be calculated using tools like hardhat-gas-reporter. In this case, we will be passing 0 as ackGasLimit because we don’t want any acknowledgement to be sent back to the source chain.
ackGasPrice: Gas price of the destination chain. This can be calculated using the RPC of source chain.In this case, we will be passing 0 as ackGasPrice because we don’t want any acknowledgement to be sent back to the source chain.
relayerFees: This is similar to priority fees that one pays on other chains. Router chain relayers execute your requests on the destination chain. So if you want your request to be picked up by relayer faster, this should be set to a higher number. If you pass really low amount, the Router chain will adjust it to some minimum amount. In this case, we will be passing 0 as relayerFees
ackType: When the contract calls have been executed on the destination chain, the iDapp has the option to get an acknowledgement back to the source chain.In this case, we will be passing 0 as ackGasPrice because we don’t want any acknowledgement to be sent back to the source chain.


Actually,we provide the option to the user to be able to get this acknowledgement from the router chain to the source chain and perform some operation based on it.
ackType = 0: You don’t want the acknowledgement to be forwarded back to the source chain.
ackType = 1: You only want to receive the acknowledgement back to the source chain in case the calls executed successfully on the destination chain and perform some operation after that.
ackType = 2: You only want to receive the acknowledgement back to the source chain in case the calls errored on the destination chain and perform some operation after that.
ackType = 3: You only want to receive the acknowledgement back to the source chain in both the cases (success and error) and perform some operation after that.
isReadCall: We provide you the option to query a contract from another chain and get the data back on the source chain through acknowledgement. If you just want to query a contract on destination chain, set this to true. In this case, we will be setting this to false because we don’t want to query the contract from another chain and get the data back on the source chain through acknowledgement.
asmAddress: We also provide modular security framework for creating an additional layer of security on top of the security provided by Router Chain. These will be in the form of smart contracts on destination chain. The address of this contract needs to be passed in the form of string in this variable.In this case, we will be passing an empty string “” in asmAddress because we don’t want any additional security.

The request metadata can be constructed using the following code:
	
```sh
function getRequestMetadata(
    uint64 destGasLimit,
    uint64 destGasPrice,
    uint64 ackGasLimit,
    uint64 ackGasPrice,
    uint128 relayerFees,
    uint8 ackType,
    bool isReadCall,
    string calldata asmAddress
  ) public pure returns (bytes memory) {
    bytes memory requestMetadata = abi.encodePacked(
      destGasLimit,
      destGasPrice,
      ackGasLimit,
      ackGasPrice,
      relayerFees,
      ackType,
      isReadCall,
      asmAddress
    );
    return requestMetadata;
  }
```




The NFTs will be transferred across chains via the burn-mint technique, in which we mint NFTs for the receiver on the destination chain after burning NFTs from the source chain's user account.

Burning is the act of sending an NFT to a null address, and minting is the act of moving a null address NFT to the address of the recipient to create a brand-new NFT.

To achieve this, we shall create a public & payable function transferCrossChain that takes three parameters. But before that lets know more about various types(public, private, external, internal, pure, view) of solidity functions from here. 


Code
	
```sh

function transferCrossChain(
    string calldata destChainId,
    TransferParams calldata transferParams,
    bytes calldata requestMetadata
  ) public payable {
    require(
      keccak256(bytes(ourContractOnChains[destChainId])) !=
        keccak256(bytes("")),
      "contract on dest not set"
    );

    require(
      _ownerOf(transferParams.nftId) == msg.sender,
      "caller is not the owner"
    );

    // burning the NFT from the address of the user calling _burn function
    _burn(transferParams.nftId);

    // sending the transfer params struct to the destination chain as payload.
    bytes memory packet = abi.encode(transferParams);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );

    gatewayContract.iSend{ value: msg.value }(
      1,
      0,
      string(""),
      destChainId,
      requestMetadata,
      requestPacket
    );
  }
```

Let us know about the stuff that's going on here step by step;

```ssh
require(
      keccak256(bytes(ourContractOnChains[destChainId])) !=
        keccak256(bytes("")),
      "contract on dest not set"
    );
	
```
Making sure the counter contract set is not empty.
	
```sh
require(
      _ownerOf(transferParams.nftId) == msg.sender,
      "caller is not the owner"
    );
```


Making sure the person calling the function is the owner of the NFT that he wants to transfer: 


// burning the NFT from the address of the user calling this function
  _burn(id);
Burning the NFTs from the user's account: Using the _burn method specified in the Openzeppelin library's ERC-721 contract, we will burn the NFTs from the user's account before generating a cross-chain communication request to the destination chain.


```sh
  bytes memory packet = abi.encode(transferParams);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );
```


We encode transferParams in a variable called packet and further encode the packet  along with the destination contract address in a variable called requestPacket.This is the data we require to execute out our function or logic at the destination chain. 
We then call the iSend function of Router’s Gateway Contract that facilitates the transmission of a cross-chain message. Whenever users want to execute a cross-chain request, they can call this function by passing the required parameters.
version: Current version of Gateway contract which can be queried from the Gateway contract using the following function.
routeAmount: If one wants to transfer Route tokens along with the call, they will have to pass the amount of tokens to be transferred here.
routeRecipient: If one wants to transfer Route tokens along with the call, they will have to pass the address of recipient on the destination chain to which Route tokens will be minted on destination chain.
destChainId: Chain ID of the destination chain in string format.
requestMetadata: Some static information for the request. This is created so that iDapps don’t have to encode it on-chain, they can just send it as a parameter to their iDapp depending on the destination chain Id passed by the user.
requestPacket: This is bytes encoded string consisting of two parameters:
a. destContractAddress: This is the address of the smart contract on the destination chain which will handle the payload that you send from the source chain to the destination chain. 
b. payload: This is bytes containing the payload that you want to send to the destination chain. This can be anything depending on your utility. In this case ,it is the recipient address on destination chain and the amount of tokens to be sent on the destination chain.


Finally, we have generated a cross-chain request that will activate the destination chain contract successfully.


Who will handle our request on the destination chain now that we have sent the request from the source chain?



The function iReceive will be developed to handle the request on the destination chain after the NFT has been burnt on the source chain and a cross-chain transfer request has been generated in the transferCrossChain function.
By handling the request, we imply that the function should be able to decode the encoded data we sent via transferCrossChain, mint the NFT for the receiver on the destination chain.
Keep in mind that the name of the function, iReceive, matters in this case. As this function is called by the Gateway contract on the destination chain, the name and the parameters it gets must correspond for the call to succeed. If the name or the parameters to this function change, the call will fail.

Remember? Initially we implemented an empty function iReceive for the sake of compiling the code at every stage, now is the time to add logic into that function. Make sure you're adding the following logic into that function only and not creating another function named iReceive


code>>>


Let’s check out what each line means;


```sh
function iReceive(
    string memory requestSender,
    bytes memory packet,
    string memory srcChainId
  ) external override returns (bytes memory) {
    require(msg.sender == address(gatewayContract), "only gateway");
    require(
      keccak256(bytes(ourContractOnChains[srcChainId])) ==
        keccak256(bytes(requestSender))
    );

    // decoding our payload
    TransferParams memory transferParams = abi.decode(packet, (TransferParams));
    string memory uri = transferParams.uri;
    safeMint(toAddress(transferParams.recipient), transferParams.nftId,uri);

    return "";
  }
```






```sh
require(msg.sender == address(gatewayContract), "only gateway");
```

Verify that the function's caller is the gateway contract that Router has deployed on the destination chain: This verification ensures that no other smart contract or externally owned account may trigger this function.


```sh
require(
      keccak256(srcContractAddress) ==
        keccak256(ourContractOnChains[srcChainId])
    );
```
Verify that our contract on the destination chain received the request from our contract on the source chain: This verification ensures that only the counterparts of our contracts on various chains can connect with one another and that no other wallets or externally owned accounts are able to interfere with the functioning.


```sh
TransferParams memory transferParams = abi.decode(packet, (TransferParams));
```


Decode the encoded data sent from the source chain: Since the request was generated by us, we know exactly what is received inside it. Since we sent the token ID,recipient address and token URI, we will decode it and store it in a transferParams variable. 

```sh
string memory uri = transferParams.uri;
safeMint(toAddress(transferParams.recipient), transferParams.nftId,uri);

    return "";
  }
	
```


Mint the NFT to recipient: Using the safemint function, we will now mint the NFT that was burned on the source chain to the recipient on the destination chain.
We need to create safeMint function separately for the above purpose

```sh
function safeMint(address to, uint256 tokenId, string memory uri) public 
    {
      // require(msg.sender == owner, "only owner");
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }
```




 safeMint function calls _safeMint function of the ERC-721 contract provided by the Openzeppelin library.


We have now completed the request handling for the destination chain. We must implement the iAck method because we descended from imported libraries or we will encounter an error.
We will only implement an empty function to handle acknowledgement because we don't want to handle it on the source chain.

```sh

 function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}
```


Smart contracts

Here is how your `XERC721.sol` should look like on the chain where we shall do the initial minting, Polygon Mumbai in our case.

```solidity
// SPDX-License-Identifier: Unlicensed
pragma solidity >=0.8.0 <0.9.0;

import "@routerprotocol/evm-gateway-contracts/contracts/IDapp.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/IGateway.sol";
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

/// @title XERC721
/// @author Yashika Goyal
/// @notice A cross-chain ERC-721 smart contract to demonstrate how one can create
/// cross-chain NFT contracts using Router CrossTalk.
contract XERC721 is ERC721,ERC721URIStorage,IDapp {
  // address of the owner
  address public owner;

  // address of the gateway contract
  IGateway public gatewayContract;

  // chain type + chain id => address of our contract in bytes
  mapping(string => string) public ourContractOnChains;

  // transfer params struct where we specify which NFT should be transferred to
  // the destination chain and to which address
  struct TransferParams {
    uint256 nftId;
    bytes recipient;
    string uri;
  }

  constructor(
    address payable gatewayAddress,
    string memory feePayerAddress
  ) ERC721("ERC721", "ERC721") {
    gatewayContract = IGateway(gatewayAddress);
    owner = msg.sender;

  
    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the fee payer address on Router Chain.
  /// @param feePayerAddress address of the fee payer on Router Chain.
  function setDappMetadata(string memory feePayerAddress) external {
    require(msg.sender == owner, "only owner");
    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the Router Gateway Contract.
  /// @param gateway address of the gateway contract.
  function setGateway(address gateway) external {
    require(msg.sender == owner, "only owner");
    gatewayContract = IGateway(gateway);
  }

function safeMint(address to, uint256 tokenId, string memory uri) public 
    {
      // require(msg.sender == owner, "only owner");
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
    }
     function tokenURI(uint256 tokenId)
        public
        view
        override(ERC721, ERC721URIStorage)
        returns (string memory)
    {
        return super.tokenURI(tokenId);
    }
  function mint(address to, uint256 tokenId,string memory uri) external {
    require(msg.sender == owner, "only owner");
        _safeMint(to, tokenId);
        _setTokenURI(tokenId, uri);
  }

  /// @notice function to set the address of our ERC20 contracts on different chains.
  /// This will help in access control when a cross-chain request is received.
  /// @param chainId chain Id of the destination chain in string.
  /// @param contractAddress address of the ERC20 contract on the destination chain.
  function setContractOnChain(
    string calldata chainId,
    string calldata contractAddress
  ) external {
    require(msg.sender == owner, "only owner");
    ourContractOnChains[chainId] = contractAddress;
  }
 function _burn(uint256 tokenId) internal override(ERC721, ERC721URIStorage) {
        super._burn(tokenId);
    }
  /// @notice function to generate a cross-chain NFT transfer request.
  /// @param destChainId chain ID of the destination chain in string.
  /// @param transferParams transfer params struct.
  /// @param requestMetadata abi-encoded metadata according to source and destination chains
  function transferCrossChain(
    string calldata destChainId,
    TransferParams calldata transferParams,
    bytes calldata requestMetadata
  ) public payable {
    require(
      keccak256(bytes(ourContractOnChains[destChainId])) !=
        keccak256(bytes("")),
      "contract on dest not set"
    );

    require(
      _ownerOf(transferParams.nftId) == msg.sender,
      "caller is not the owner"
    );

    // burning the NFT from the address of the user calling _burn function
    _burn(transferParams.nftId);

    // sending the transfer params struct to the destination chain as payload.
    bytes memory packet = abi.encode(transferParams);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );

    gatewayContract.iSend{ value: msg.value }(
      1,
      0,
      string(""),
      destChainId,
      requestMetadata,
      requestPacket
    );
  }

  /// @notice function to get the request metadata to be used while initiating cross-chain request
  /// @return requestMetadata abi-encoded metadata according to source and destination chains
  function getRequestMetadata(
    uint64 destGasLimit,
    uint64 destGasPrice,
    uint64 ackGasLimit,
    uint64 ackGasPrice,
    uint128 relayerFees,
    uint8 ackType,
    bool isReadCall,
    string calldata asmAddress
  ) public pure returns (bytes memory) {
    bytes memory requestMetadata = abi.encodePacked(
      destGasLimit,
      destGasPrice,
      ackGasLimit,
      ackGasPrice,
      relayerFees,
      ackType,
      isReadCall,
      asmAddress
    );
    return requestMetadata;
  }

  /// @notice function to handle the cross-chain request received from some other chain.
  /// @param requestSender address of the contract on source chain that initiated the request.
  /// @param packet the payload sent by the source chain contract when the request was created.
  /// @param srcChainId chain ID of the source chain in string.
  function iReceive(
    string memory requestSender,
    bytes memory packet,
    string memory srcChainId
  ) external override returns (bytes memory) {
    require(msg.sender == address(gatewayContract), "only gateway");
    require(
      keccak256(bytes(ourContractOnChains[srcChainId])) ==
        keccak256(bytes(requestSender))
    );

    // decoding our payload
    TransferParams memory transferParams = abi.decode(packet, (TransferParams));
    string memory uri = transferParams.uri;
    safeMint(toAddress(transferParams.recipient), transferParams.nftId,uri);

    return "";
  }

  /// @notice function to handle the acknowledgement received from the destination chain
  /// back on the source chain.
  /// @param requestIdentifier event nonce which is received when we create a cross-chain request
  /// We can use it to keep a mapping of which nonces have been executed and which did not.
  /// @param execFlag a boolean value suggesting whether the call was successfully
  /// executed on the destination chain.
  /// @param execData returning the data returned from the handleRequestFromSource
  /// function of the destination chain.
  function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}

  /// @notice Function to convert bytes to address
  /// @param _bytes bytes to be converted
  /// @return addr address pertaining to the bytes
  function toAddress(bytes memory _bytes) internal pure returns (address addr) {
    bytes20 srcTokenAddress;
    assembly {
      srcTokenAddress := mload(add(_bytes, 0x20))
    }
    addr = address(srcTokenAddress);
  }
}

```

We will be using the same contract for the destination contract as well ,so no need of writing the contract again for the destination chain.

Congratulations for completing the contracts for your cross-chain NFTs!


As you're now ready to build cross-chain contracts using Router's Crosstalk Utils, your challenge is to create a similar cross-chain NFT, named as `XERC1155.sol`, build upon ERC1155 standard.



```// SPDX-License-Identifier: Unlicensed
pragma solidity >=0.8.0 <0.9.0;

import "@routerprotocol/evm-gateway-contracts/contracts/IDapp.sol";
import "@routerprotocol/evm-gateway-contracts/contracts/IGateway.sol";
import "@openzeppelin/contracts/token/ERC1155/ERC1155.sol";

/// @title XERC1155
/// @author Yashika Goyal
/// @notice A cross-chain ERC-1155 smart contract to demonstrate how one can create
/// cross-chain NFT contracts using Router CrossTalk.
contract XERC1155 is ERC1155, IDapp {
  // address of the owner
  address public owner;

  // address of the gateway contract
  IGateway public gatewayContract;

  // chain type + chain id => address of our contract in bytes
  mapping(string => string) public ourContractOnChains;

  // transfer params struct where we specify which NFTs should be transferred to
  // the destination chain and to which address
  struct TransferParams {
    uint256[] nftIds;
    uint256[] nftAmounts;
    bytes nftData;
    bytes recipient;
  }

  constructor(
    string memory _uri,
    address payable gatewayAddress,
    string memory feePayerAddress
  ) ERC1155(_uri) {
    gatewayContract = IGateway(gatewayAddress);
    owner = msg.sender;

    // minting ourselves some NFTs so that we can test out the contracts
    _mint(msg.sender, 1, 10, "");

    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the fee payer address on Router Chain.
  /// @param feePayerAddress address of the fee payer on Router Chain.
  function setDappMetadata(string memory feePayerAddress) external {
    require(msg.sender == owner, "only owner");
    gatewayContract.setDappMetadata(feePayerAddress);
  }

  /// @notice function to set the Router Gateway Contract.
  /// @param gateway address of the gateway contract.
  function setGateway(address gateway) external {
    require(msg.sender == owner, "only owner");
    gatewayContract = IGateway(gateway);
  }

  function mint(
    address account,
    uint256[] memory nftIds,
    uint256[] memory amounts,
    bytes memory nftData
  ) external {
    require(msg.sender == owner, "only owner");
    _mintBatch(account, nftIds, amounts, nftData);
  }

  /// @notice function to set the address of our NFT contracts on different chains.
  /// This will help in access control when a cross-chain request is received.
  /// @param chainId chain Id of the destination chain in string.
  /// @param contractAddress address of the NFT contract on the destination chain.
  function setContractOnChain(
    string calldata chainId,
    string calldata contractAddress
  ) external {
    require(msg.sender == owner, "only owner");
    ourContractOnChains[chainId] = contractAddress;
  }

  /// @notice function to generate a cross-chain NFT transfer request.
  /// @param destChainId chain ID of the destination chain in string.
  /// @param transferParams transfer params struct.
  /// @param requestMetadata abi-encoded metadata according to source and destination chains
  function transferCrossChain(
    string memory destChainId,
    TransferParams memory transferParams,
    bytes memory requestMetadata
  ) public payable {
    require(
      keccak256(bytes(ourContractOnChains[destChainId])) !=
        keccak256(bytes("")),
      "contract on dest not set"
    );

    // burning the NFTs from the address of the user calling _burnBatch function
    _burnBatch(msg.sender, transferParams.nftIds, transferParams.nftAmounts);

    // sending the transfer params struct to the destination chain as payload.
    bytes memory packet = abi.encode(transferParams);
    bytes memory requestPacket = abi.encode(
      ourContractOnChains[destChainId],
      packet
    );

    gatewayContract.iSend{ value: msg.value }(
      1,
      0,
      string(""),
      destChainId,
      requestMetadata,
      requestPacket
    );
  }

  /// @notice function to get the request metadata to be used while initiating cross-chain request
  /// @return requestMetadata abi-encoded metadata according to source and destination chains
  function getRequestMetadata(
    uint64 destGasLimit,
    uint64 destGasPrice,
    uint64 ackGasLimit,
    uint64 ackGasPrice,
    uint128 relayerFees,
    uint8 ackType,
    bool isReadCall,
    bytes memory asmAddress
  ) public pure returns (bytes memory) {
    bytes memory requestMetadata = abi.encodePacked(
      destGasLimit,
      destGasPrice,
      ackGasLimit,
      ackGasPrice,
      relayerFees,
      ackType,
      isReadCall,
      asmAddress
    );
    return requestMetadata;
  }

  /// @notice function to handle the cross-chain request received from some other chain.
  /// @param requestSender address of the contract on source chain that initiated the request.
  /// @param packet the payload sent by the source chain contract when the request was created.
  /// @param srcChainId chain ID of the source chain in string.
  function iReceive(
    string calldata requestSender,
    bytes calldata packet,
    string calldata srcChainId
  ) external override returns (bytes memory) {
    require(msg.sender == address(gatewayContract), "only gateway");
    require(
      keccak256(bytes(ourContractOnChains[srcChainId])) ==
        keccak256(bytes(requestSender))
    );

    // decoding our payload
    TransferParams memory transferParams = abi.decode(packet, (TransferParams));
    _mintBatch(
      toAddress(transferParams.recipient),
      transferParams.nftIds,
      transferParams.nftAmounts,
      transferParams.nftData
    );

    return "";
  }

  /// @notice function to handle the acknowledgement received from the destination chain
  /// back on the source chain.
  /// @param requestIdentifier event nonce which is received when we create a cross-chain request
  /// We can use it to keep a mapping of which nonces have been executed and which did not.
  /// @param execFlag a boolean value suggesting whether the call was successfully
  /// executed on the destination chain.
  /// @param execData returning the data returned from the handleRequestFromSource
  /// function of the destination chain.
  function iAck(
    uint256 requestIdentifier,
    bool execFlag,
    bytes memory execData
  ) external override {}

  /// @notice Function to convert bytes to address
  /// @param _bytes bytes to be converted
  /// @return addr address pertaining to the bytes
  function toAddress(bytes memory _bytes) internal pure returns (address addr) {
    bytes20 srcTokenAddress;
    assembly {
      srcTokenAddress := mload(add(_bytes, 0x20))
    }
    addr = address(srcTokenAddress);
  }
}
```







**Deployment**

Now, we will start deployment;

Compile your XERC721.sol contract using the Solidity Compiler.


After successful compilation, go to deploy and run Transactions window Select Injected Provider - Metamask under Environment and connect your metamask to Polygon Mumbai network.



Deploy the contract XERC721.sol in which we were passing the Constructor arguments: your NFT's name and symbol, the feePayer Address, and the gateway address for the Polygon Mumbai testnet. The gateway contracts' addresses can be found here.Confirm the transaction that appears on metamask after you transact.



Go to the Deploy and run transactions window and connect your metamask to the Avalanche Fuji network .


Deploy the contract similarly by passing the constructor arguments in the similar manner we did for the contract on Mumbai. Make sure to pass the gateway address for Fuji testnet here.




And the deployment of our contracts is complete. If you scroll down through the DEPLOY AND RUN TRANSACTIONS Tab, you can see instances of your deployed contracts that look like this. 

It’s time to mint your NFT on the source chain !!
1. Connect your Metamask to Mumbai Network
2. Go to Deployed Contracts and choose the first  instance
3. Go to the mint function and pass in the address you wanted the NFT to get minted to, Id you want the NFT  to have and the URI for your NFT.

Here's news, you can also see your NFTs that you minted to yourself on Polygon Mumbai.By utilising the ownerOf function, you may determine who owns the token IDs .
Your address must appear in the output, according to this.


Then we also need to set the destination contract on the source chain and via versa.

Connect the Polygon Mumbai network with your metamask.
Make sure you've opened the first under Deployed contracts in Remix in the manner described above. You can access all the functions we created and those our contract has inherited from other contracts here.


Transact by providing the Chain ID's, and address of the contract that we deployed on the Fuji network to the setContractOnChain function
Put 43113 as the chain Id, and the address of the  contract that we deployed on Fuji.




Connect your metamask to the Avalanche Fuji network after this step.
Ensure that the contract we deployed on Avalanche Fuji is open in Remix under Deployed contracts.
Transact by supplying the chain ID, and address of the contract that we placed on the Mumbai network to the setContractOnChain function. As an example, enter 80001 as the chain ID, and the address of the contract that was set up in Mumbai. 




Now we will test our Cross chain NFT Smart contracts.
We can finally see the results of our contract now. 

Follow the steps;

Connect your metamask to Polygon Mumbai network.


Make sure you have opened the contract that we deployed on Polygon Mumbai under Deployed contracts in Remix.


Get the requestMetadata by passing necessary parameters in getRequestMetadata function as discussed earlier.



Transact by passing the  chain Id, transferParams and requestMetadata to the transferCrossChain function .
Since, tranferParams is a struct 

```
struct TransferParams {
    uint256 nftId;
    bytes recipient;
    string uri;
  }
```
We need to pass in this format :-
[<nftId>,<recipient address>,<uri>]
	
Visit Router Explorer and click on Fee Payer option present in More section.Connect your metamask to Router Testnet .Find your dapp address there and click on approve button corresponding to your dapp to approve it.



Once, the approval is done, go to CROSSCHAIN . You can track the status of your transaction there. Wait for sometime till your transaction gets executed.



Go to your Mumbai instance in Remix and call the function ownerOf like we did before while your metamask is still attached to Polygon Mumbai. Verify who the owner of the NFT you just sent through transferCrossChain function. Your address wouldn't be in the output, though. It indicates your NFT was indeed burnt in Mumbai and is on its way to you on Fuji.


Now, connect the Avalanche Fuji network using your metamask, access your Fuji instance, and use the same procedure as before to use the ownerOf method. Verify who the owner of the NFT you just sent through transferCrossChain function from the Mumbai instance of the contract.Your address needs to appear in the output, which you can see. It indicates that you have received your burnt NFT from Mumbai on Fuji.


You should be able to see that it actually got transferred on Fuji network.


# `Projects ideas`

**Voyager Ideas:**
Cross-Chain swaps are one of the most important utilities of a cross-chain ecosystem. Seamlessly swapping arbitrary assets within chains open new doors. You can leverage cross-chain swaps in a box to build new DApps. 

**Cross-chain Lending front end and primitive**
Money market lending protocol native to Router Chain (akin to Compound or Aave). Think of optimizations in a cross-chain environment. Users could extract maximum yields and pay minimum interest! 

**Cross-Chain Front runner**
A protocol that immediately provides immediate liquidity for assets transferred cross-chain,  such that users don't need to wait for the cross-chain assets to be fully confirmed on both chains - in return for a premium fee. Optimistic bridging, anyone?

**Fixed-Rate Loans**
Where users can take out a fixed-rate loan on an Arbitrum Lending/Borrowing protocol that is enabled by an on-chain interest rate swap order book on Polygon

**Faucet**
build an open-source service that enables ROUTE airdrops for first-time Router users using different rate-limiting mechanisms. Think of a Sybil-resistant faucet.  

**Cross-Chain Stablecoin**
Think of a new primitive to create the world’s first decentralized, 100% collateralized natively cross-chain stablecoin! 

**Cross-chain NFT Marketplace**
Allow buying any NFT from any chain and selling any NFT on any chain. The markets should be focussed on natively cross-chain NFTs.

**Decentralized Cross-chain Identity**
Decentralized Cross-chain Identity Aggregator that enables linking user identities across multiple networks on the Router chain. Think of a cross-chain ENS where identities are mapped of all networks to single .ROUTE address. 

**Cross-Chain Insurance markets**
A decentralized market where anyone can insure a cross-chain transaction. Can start from Router Protocol insurance and later extend to all cross-chain protocols. 

**On-chain subscriptions**
A protocol that is a factory for any app to create on-chain payment streaming and subscriptions in a cross-chain environment. The creators can take funds on any chain - the subscribers can pay from any chain. Abstract all the complexities. 

**Education** 
A game-type protocol that walks a user thu the interoperable journey. Combining video clips, interoperability resources and cross-chain swaps & NFTs  quest-type journey to get normies up to speed

**Cross-Chain Arbitrage bots**
A bot that automatically captures arbitrage in low liquidity cross-chain environment. The bot should use Voyager for cross-chain swaps.

**Oracles**
Create a fully on-chain cross-chain pricing oracle.

**Slashing Insurance**
A simple insurance protocol where the Router-chain community can provide insurance for validators against the risk of being slashed

**Cross-Chain Yield Aggregator**: Simplify staking across multiple chains by allowing users to view and stake from any chain they have funds on, eliminating the need for asset transfers.

**Cross-Chain Disperse Application**: Create a DApp where users can disperse funds to multiple chains in a single transaction by specifying addresses, chains, and amounts, using Router Protocol.

**Cross-Chain Governance**: Develop a cross-chain governance mechanism for simultaneous voting across multiple chains, ensuring fair decision-making and using middleware contracts to add additional voting power based on specific requirements.

**Cross Chain Liquidity Aggregator**: Build a system that consolidates liquidity from different chains into Router Chain, enhancing trading efficiency and reducing fragmentation across decentralized exchanges.

**Omnichain SBTs**: Enable Sould Bound Tokens (SBTs) to be seen and verified across any chain by leveraging Router Chain, enhancing their accessibility and usability.

**Cross-chain social media platforms using Router Protocol**: Build a social media platform that allows users to interact with smart contracts on a chain with lower transaction fees while securely storing posts on a more secure chain.





