class: center, middle

# Smart Contract and Solidity

Reference: http://solidity.readthedocs.io/en/latest

---
## Introduction to smart contract

A contract in the sense of Solidity is **a collection of code** (its functions) and **data** (its state) that resides at a specific **address** on the Ethereum blockchain.

```Javascript
contract OneValue {
    uint256 value;

    function OneValue(uint256 initValue) {
        value = initValue;
    }

    function setValue(uint256 v) {
        value = v;
    }

    function getValue() constant returns (uint256 result){
        return value;
    }
}
```

Deployed in testnet at [`0xd34d02bd80b7ad234305e734bd229746e5fe204e`](http://testnet.etherscan.io/address/0xd34d02bd80b7ad234305e734bd229746e5fe204e)

---
## Ethereum Virtual Machine

- The Ethereum Virtual Machine or EVM is the runtime environment for smart contracts in Ethereum.

---
## Accounts
- External accounts: controlled by public-private key pairs (i.e. humans)

- Contract accounts: controlled by the code stored together with the account

- EVM treats the two types equally

---
## Transactions
- A blockchain is a globally shared, transactional database

- Everyone can read

- To change anything in the database, you need to create a transaction

- A transaction is a description of how to update the database

---
## Transactions

A transaction is a **message** that is sent from **one account to another account** (which might be the same or the special zero-account, see below). It can include **binary data** (its payload) and **Ether**.

- Send ether value

- Create / deploy contract

- Send contract tx (to update storage)

---
## Transactions: send ether value

```Javascript
addr = eth.accounts[0];
to = eth.accounts[1];
personal.unlockAccount(addr, 'test1234');
eth.sendTransaction(
    {from:addr, to:to, value:web3.toWei(1.123, "ether")});
```

Example transaction: [`0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f`](http://testnet.etherscan.io/tx/0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f)

---
## Transactions: send ether value
```Javascript
> eth.getTransaction("0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f")
{
  blockHash: "0x1b18a2ea2c63991c05232c289c19be0dcfe463edf76be0fb63e5ac0847d621ca",
  blockNumber: 396919,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gas: 90000,
  gasPrice: 20000000000,
  hash: "0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f",
  input: "0x",
  nonce: 11,
  r: "0x5b10648e0aae6c85b74cd0bca90c04e97ad5672f0a7cd7a42602145b10ec080a",
  s: "0x3b5f971a4b0ca412188ee07d7b69cfb7b0c80e6854b0bf80e7e61d3bc4dcf7d3",
  to: "0x32124ceb661133adabf63189b5950394bbc3e922",
  transactionIndex: 0,
  v: "0x2a",
  value: 1123000000000000000
}
```

---
## Transactions: send ether value

- gasUsed: `21000`

```Javascript
> eth.getTransactionReceipt("0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f")
{
  blockHash: "0x1b18a2ea2c63991c05232c289c19be0dcfe463edf76be0fb63e5ac0847d621ca",
  blockNumber: 396919,
  contractAddress: null,
  cumulativeGasUsed: 21000,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gasUsed: 21000,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  root: "0xfa81c915ca3c1a0ae5b565360906046c17da5412d5f20201f3a4ffddc80937df",
  to: "0x32124ceb661133adabf63189b5950394bbc3e922",
  transactionHash: "0xc7e9557e1a97aa4b6cbcd0dd07379121730a68de2bde67b9d5815a69b35a250f",
  transactionIndex: 0
}
```

---
## Transactions: create / deploy contract
```bash
node deploy-contract-test.js
```

```Javascript
var abi  = ...
var evmCode = ...
var OneValueContract = eth.contract(abi);
var initValue = 123;
var oneValue = OneValueContract.new(initValue,
    {from:addr, data: evmCode, gas: 200000},
    function(error, contract){
        ...
    }
);
//After mined
oneValue.getValue();
```

Example transaction: [`0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2`](http://testnet.etherscan.io/tx/0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2)


---
## Transactions: create / deploy contract

- to: `null`

```Javascript
> eth.getTransaction("0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2")
{
  blockHash: "0xed9ebcd17447713ccd2a4ef4c4afc1a59ebb14d49a4e9c43d61f02704717e199",
  blockNumber: 396928,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gas: 200000,
  gasPrice: 20000000000,
  hash: "0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2",
  input: "0x606060405260405160208060cf833981016040528080519060200190919050505b806000600050819055505b5060978060386000396000f360606040526000357c010000000000000000000000000000000000000000000000000000000090048063209652551460415780635524107714606257603f565b005b604c60048050506078565b6040518082815260200191505060405180910390f35b607660048080359060200190919050506089565b005b600060006000505490506086565b90565b806000600050819055505b5056000000000000000000000000000000000000000000000000000000000000007b",
  nonce: 12,
  r: "0xa348e9c69b0b8c093b1b2fb3f7c211bd3bc1b57922f9d0e0dccd2bcab755f727",
  s: "0x3b891da5afdeb7b8bcaf8e59e65f46bbc517f4af000e0a2a674e480829ec28f2",
  to: null,
  transactionIndex: 1,
  v: "0x29",
  value: 0
}
```
---
## Transactions: create / deploy contract

contractAddress:   [`0xd34d02bd80b7ad234305e734bd229746e5fe204e`](http://testnet.etherscan.io/address/0xd34d02bd80b7ad234305e734bd229746e5fe204e)

```JavaScript
> eth.getTransactionReceipt("0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2")
{
  blockHash: "0xed9ebcd17447713ccd2a4ef4c4afc1a59ebb14d49a4e9c43d61f02704717e199",
  blockNumber: 396928,
  contractAddress: "0xd34d02bd80b7ad234305e734bd229746e5fe204e",
  cumulativeGasUsed: 353543,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gasUsed: 115055,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  root: "0xf86e5dfca0a372d2adbf9c1e91dd12edc653ffe93195dd42fd56fcbb14380ec3",
  to: null,
  transactionHash: "0xc4974221fedc7455c68d980b396ae9a2dd210bb149f448c0d56bea15badd98b2",
  transactionIndex: 1
}
```

---
## Transactions: send contract tx
```bash
node update-contract-test.js
```

```Javascript
var txid = oneValue.setValue.sendTransaction(1234,
    {from:addr, to:oneValue.address, gas:200000});
eth.getTransation(txid);
//After mined
eth.getTransationReceipt(txid);
oneValue.getValue();
```

Example transaction: [`0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276`](http://testnet.etherscan.io/tx/0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276)

---
## Transactions: send contract tx

```Javascript
> eth.getTransaction("0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276")
{
  blockHash: "0xee094716246b40255460023aaa0d596eae4daab4b466b93f8a1ddbdcfe945784",
  blockNumber: 396938,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gas: 90000,
  gasPrice: 20000000000,
  hash: "0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276",
  input: "0x5524107700000000000000000000000000000000000000000000000000000000000004d2",
  nonce: 13,
  r: "0x29195d5d5d89e3fc188f859f184e52c4c5db0d78727bfff16d22df9eee31f47f",
  s: "0x73c3a700c42683f76c39cc81d163dd734795aef86701d44fab855e8f96b29d44",
  to: "0xd34d02bd80b7ad234305e734bd229746e5fe204e",
  transactionIndex: 2,
  v: "0x2a",
  value: 0
}
```

---
## Transactions: send contract tx

```Javascript
> eth.getTransactionReceipt("0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276")
{
  blockHash: "0xee094716246b40255460023aaa0d596eae4daab4b466b93f8a1ddbdcfe945784",
  blockNumber: 396938,
  contractAddress: null,
  cumulativeGasUsed: 318120,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gasUsed: 26688,
  logs: [],
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  root: "0x726e6c060c151429563f3fa6748a43bffb547716fca29ae4485dacfa149bec7f",
  to: "0xd34d02bd80b7ad234305e734bd229746e5fe204e",
  transactionHash: "0xf9132a6811770ffdecb5bd9ea9decc15beb66532f97d8ea0bca4f44cc135c276",
  transactionIndex: 2
}
```

---
## Gas
- `fee = gas * gasPrice`

- protocol_params.go:
    ```go
    TxGasContractCreation = big.NewInt(53000)
    // Per transaction that creates a contract.
    ```
- Current gas price: [EthStats](https://ethstats.net/), [EtherScan](https://etherscan.io/chart/gasprice)
    ```Javascript
    20 gwei = 20_000_000_000 wei (10**9)
    -> Create transaction take (53000 * 20_000_000_000) wei
    -> 0.00106 ether
    ```
- Transaction tracking: [EtherScan](https://etherscan.io), [BlockSeer](https://www.blockseer.com/eth)

---
## Message calls

- Contracts can call other contracts or send Ether to non-contract accounts by the means of message calls.

---
## Logs

- This feature called logs is used by Solidity in order to implement events. See logs field in eth.getTransactionReceipt.

---
class: middle, center
# Structure and types

---
## Contract structure

Contracts in Solidity are similar to classes in object-oriented languages.

- State Variables
- Functions
- Function Modifiers
- Events
- Structs
- Arrays
- Mappings
- Enum

---
## State variables
- State variables are values which are permanently stored in contract storage.

```Javascript
contract SimpleStorage {
    uint storedData; // State variable
    // ...
}
```

---
## Functions
- Functions are the executable units of code within a contract.

```Javascript
contract SimpleAuction {
    function bid() { // Function
        // ...
    }
}
```

---
## Function modifiers
- Function modifiers can be used to amend the semantics of functions in a declarative way.

```Javascript
contract Purchase {
    address public seller;

    modifier onlySeller { // Modifier
        if (msg.sender != seller) throw;
        _;
    }

    function abort() onlySeller { // Modifier usage
        // ...
    }
}
```

---
## Events
- Events are convenience interfaces with the EVM logging facilities.

```Javascript
contract SimpleAuction {
    event HighestBidIncreased(address bidder, uint amount); // Event

    function bid() {
        // ...
        HighestBidIncreased(msg.sender, msg.value); // Triggering event
    }
}
```

---
## Structs
- Structs are custom defined types that can group several variables (see Structs in types section).

```Javascript
contract Ballot {
    struct Voter { // Struct
        uint weight;
        bool voted;
        address delegate;
        uint vote;
    }
}
```

---
## Arrays
- An array of fixed size k and element type `T` is written as `T[k]`, an array of dynamic size as `T[]`.

Array members: `length`, `push`

```JavaScript
contract C {
    function f(uint len) {
        uint[] memory a = new uint[](7);
        bytes memory b = new bytes(len);
        // Here we have a.length == 7 and b.length == len
        a[6] = 8;
    }
}
```

---

## Mappings
- Mapping types are declared as mapping `_KeyType => _ValueType`
    - `_KeyType` can be almost any type except for a mapping, a dynamically sized array, a contract, an enum and a struct.
    - `_ValueType` can actually be any type, including mappings.

```Javascript
//Ledger
mapping (address => uint) balances;
```

---

## Enum types

- Enums are one way to create a user-defined type in Solidity. They are explicitly convertible to and from all integer types but implicit conversion is not allowed.

```Javascript
contract Purchase {
    enum State { Created, Locked, Inactive } // Enum
}
```

---
## Types
- Solidity is a statically typed language.

    - Value types

    - Function types

    - Reference types

---
## Value types
- Boolean: `bool`
- Integers: `uint8`, `uint16`, ..., `uint256`, `int8`, `int16`, ..., `int256`
    `uint` is alias of `uint256`, and `int` is alias of `int256`
- Address: `address`, 20 byte value.    
- Fixed-size byte arrays
    `bytes1`, `bytes2`, `bytes3`, ..., `bytes32`. byte is an alias for `bytes1`
- Enum types
- Rational number and more
    http://solidity.readthedocs.io/en/latest/types.html#value-types

---
## Reference types:
- Complex type. Data not fit in 256 bits
- Arrays:
    - Value type arrays: fixed or dynamic length
    - `bytes`: dynamically-sized byte array
    - `string`: dynamically-sized UTF-8-encoded string
- Structs
- Mappings

---

## Data location: memory and storage
- Memory: not persisting.
    - function arguments
    - return parameters
- Storage: where the state variables are held
    - state variables
    - local variables
- Assignments between storage and memory and also to a state variable always create an independent copy.

---

## Assignments of Arrays and Structs
- Assigning to a state variable always creates an independent copy.
- Assigning to a local variable creates an independent copy only for elementary types.
- Assigning structs and arrays from a state variable to a local variable, the local variable holds a reference.
    - A second assignment only modify the reference and not change the state.
    - Assignments to members (or elements) do change the state.

---

## Data location: summary
- Forced data location:
    - parameters (not return) of external functions: calldata
    - state variables: storage
- Default data location:
    - parameters (also return) of functions: memory
    - all other local variables: storage

---

## Function types:

- Two flavors: `internal` and `external` functions:
    - Internal functions: can only be used inside the current contract.
    - External functions: consist of an address and a function signature and can be passed via and returned from external function calls.
- Notation:
    ```Javascript
    function (<parameter types>) {internal|external} [constant] [payable] [returns (<return types>)]
    ```

---
## Function types:

```Solidity
library ArrayUtils {
    function map(uint[] memory self, function (uint) returns (uint) f)
      internal returns (uint[] memory r) {
      r = new uint[](self.length);
      for (uint i = 0; i < self.length; i++) {
        r[i] = f(self[i]);
      }
    }
    function reduce( uint[] memory self, function (uint x, uint y) returns (uint) f)
      internal returns (uint r) {
      r = self[0];
      for (uint i = 1; i < self.length; i++) {
        r = f(r, self[i]);
      }
    }
    function range(uint length) internal returns (uint[] memory r) {
      r = new uint[](length);
      for (uint i = 0; i < r.length; i++) {
        r[i] = i;
      }
    }
}
```

---
## Function types:
```Javascript
contract Pyramid {
  using ArrayUtils for *;
  function pyramid(uint l) returns (uint) {
    return ArrayUtils.range(l).map(square).reduce(sum);
  }
  function square(uint x) internal returns (uint) {
    return x * x;
  }
  function sum(uint x, uint y) internal returns (uint) {
    return x + y;
  }
}
```

---

## Conversions
- Implicit Conversions
    uint8 is convertible to uint16 and int128 to int256, but int8 is not convertible to uint256
- Explicit Conversions
    ```Javascript
    uint32 a = 0x12345678;
    uint16 b = uint16(a); // b will be 0x5678 now
    ```

---
## Type deduction
```Javascript
uint24 x = 0x123;
var y = x;
```

Note: The type is only deduced from the first assignment, so the loop in the following snippet is infinite, as i will have the type uint8 and any value of this type is smaller than 2000.

```Javascript
for (var i = 0; i < 2000; i++) {
    ...
}
```

---
## Global variables
Full list: http://solidity.readthedocs.io/en/latest/units-and-global-variables.html

- Most frequently used global variables:

    - `msg.sender (address)`: sender of the message (current call)

    - `msg.value (uint)`: number of wei sent with the message

---
class: center, middle

# Expressions and Control Structures

---

## Control structures
- Most of the control structures from C/JavaScript are available in Solidity except for `switch` and `goto`.

- So there is: `if`, `else`, `while`, `for`, `break`, `continue`, `return`, `? :`, with the usual semantics known from C or JavaScript.

---
## Visibility and accessors
- `external`: 僅供外部使用，內部需使用的話要加 this. `this.f()`
- `public`: 供內外部使用
- `internal`: 僅內部使用
- `private`: 僅內部使用，繼承的合約也不能使用
- Function defaults to `public`
- State variable defaults to `internal`

---
## Visibility and accessors syntax

- The visibility specifier is given after the type for state variables and between parameter list and return parameter list for functions.

```Javascript
contract C {
    function f(uint a) private returns (uint b) { return a + 1; }
    function setData(uint a) internal { data = a; }

    uint public data;
}
```

---
## Internal function calls

```Javascript
contract C {
    function g(uint a) returns (uint ret) { return a; }
    function f() returns (uint ret) { return g(7); }
}
```

---
## External function calls

When calling functions of other contracts, the amount of Wei sent with the call and the gas can be specified:

```Javascript
contract InfoFeed {
    function info() payable returns (uint ret) { return 42; }
}


contract Consumer {
    InfoFeed feed;
    function setFeed(address addr) { feed = InfoFeed(addr); }
    function callFeed() { feed.info.value(10).gas(800)(); }
}
```

Be careful about the fact that `feed.info.value(10).gas(800)` only (locally) sets the value and amount of gas sent with the function call and only the parentheses at the end perform the actual call.

Note: The keyword `payable` is required for the function to be able to receive Ether.

---
## Creating contracts via new

```Javascript
contract D {
    uint x;
    function D(uint a) {
        x = a;
    }
}
contract C {
    D d = new D(4); // will be executed as part of C's constructor

    function createD(uint arg) {
        D newD = new D(arg);
    }

    function createAndEndowD(uint arg, uint amount) {
        // Send ether along with the creation
        D newD = (new D).value(amount)(arg);
    }
}
```

---
## Exceptions
- Exceptions make currently executing call is stopped and reverted.

```Javascript
contract Sharer {
    function sendHalf(address addr) returns (uint balance) {
        if (!addr.send(msg.value / 2))
            throw; // also reverts the transfer to Sharer
        return this.balance;
    }
}
```

---
## Automatic exceptions (1/2)
- Access an array beyond its length (i.e. x[i] where i >= x.length)
- Access a fixed-length bytesN at a too large or negative index
- Function called via a message call does not finish properly (i.e. it runs out of gas or throws an exception itself)
- Create a contract using the `new` keyword but the contract creation does not finish properly
- Divide or modulo by zero (e.g. `5 / 0` or `23 % 0`)
- Shift by a negative amount

---

## Automatic exceptions (2/2)
- Convert a value too big or negative into an enum type
- Perform an external function call targeting a contract that contains no code
- Contract receives Ether via a public function without payable modifier (including the constructor and the fallback function)
- Contract receives Ether via a public accessor function
- Calling a zero-initialized variable of internal function type
- A `.transfer()` fails

---
## No exceptions for `send` and low-level calls

- Return `false` in case of an exception:

    - `send`

    - `delegatecall`

    - `callcode`

- To transfer fund, use `transfer` instead

---
## Fallback function
- An unnamed function having no arguments.

- Will be called when:

    1. None of the other functions matches  the given function identifier.

    2. The contract receives plain Ether (without data).

---
## Fallback function example

```Javascript
contract Test {
    function() { x = 1; }
    uint x;
}

// This contract rejects any Ether sent to it. It is good
// practice to include such a function for every contract
// in order not to lose Ether.
contract Rejector {
    function() { throw; }
}

contract Caller {
    function callTest(address testAddress) {
        Test(testAddress).call(0xabcde01); // hash does not exist
        // results in Test(testAddress).x becoming == 1.
        Rejector r = Rejector(0x123);
        r.send(2 ether);
        // results in r.balance == 0
    }
}
```

---

## Fallback function warning

- Contracts that **receive Ether but do not define a fallback function throw an exception**, sending back the Ether (this was different before Solidity v0.4.0). So if you want your contract to receive Ether, you have to implement a `fallback function` (along with `payable` modifier).

---
## Events
- EVM logging
- Up to **three** parameters can receive the attribute `indexed`.
- All non-indexed arguments will be stored in the data part of the log.

```JavaScript
contract ClientReceipt {
    event Deposit(
        address indexed _from,
        bytes32 indexed _id,
        uint _value
    );

    function deposit(bytes32 _id) {
        Deposit(msg.sender, _id, msg.value);
    }
}
```

---
## Watch event with web3

```bash
node watch-client-receipt.js
```

``` Javascript
var abi = /* abi as generated by the compiler */;
var ClientReceipt = web3.eth.contract(abi);
var clientReceipt = ClientReceipt.at(0x123 /* address */);
var event = clientReceipt.Deposit();

// watch for changes
event.watch(function(error, result){
    // result will contain various information
    // including the arguments given to the Deposit
    // call.
    console.log(result);
});

// Or pass a callback to start watching immediately
var event = clientReceipt.Deposit(function(error, result) {
    console.log(result);
});
```

Example tx: [`0x1119db10a1efd36dc386b40ad20946937d70d00ece3b6ebea59b4a287fae3a65`](https://testnet.etherscan.io/tx/0x1119db10a1efd36dc386b40ad20946937d70d00ece3b6ebea59b4a287fae3a65)

---

## Event transaction receipt

```JavaScript
> eth.getTransactionReceipt("0x1119db10a1efd36dc386b40ad20946937d70d00ece3b6ebea59b4a287fae3a65")
{
  blockHash: "0xa9f8121f9294e26da1b12254bede95801fbcbf6764e85b80c2b8e1301ff79218",
  blockNumber: 506125,
  contractAddress: null,
  cumulativeGasUsed: 2779639,
  from: "0xf868d5b4a75a124f40e70b56ae5fa4dd9f802aa1",
  gasUsed: 23450,
  logs: [{
      address: "0x74e88c1ad4b66cd76bacf7165da056a87e483e87",
      blockHash: "0xa9f8121f9294e26da1b12254bede95801fbcbf6764e85b80c2b8e1301ff79218",
      blockNumber: 506125,
      data: "0x0000000000000000000000000000000000000000000000000000000000000000",
      logIndex: 6,
      removed: false,
      topics: ["0x19dacbf83c5de6658e14cbf7bcae5c15eca2eedecf1c66fbca928e4d351bea0f", "0x000000000000000000000000f868d5b4a75a124f40e70b56ae5fa4dd9f802aa1", "0x7b00000000000000000000000000000000000000000000000000000000000000"],
      transactionHash: "0x1119db10a1efd36dc386b40ad20946937d70d00ece3b6ebea59b4a287fae3a65",
      transactionIndex: 12
  }],
  logsBloom: "0x00000000000000000000000000010100000004000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000100000000002000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000001000000000000000000000000000010010000000000000000000000000400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  root: "0xca92a0b372665b200770fa28ff047f46e304a375f681ee9b9353ecc5b2716a0a",
  to: "0x74e88c1ad4b66cd76bacf7165da056a87e483e87",
  transactionHash: "0x1119db10a1efd36dc386b40ad20946937d70d00ece3b6ebea59b4a287fae3a65",
  transactionIndex: 12
}
```

---
## Inheritance
- Solidity supports multiple inheritance by copying code including polymorphism.

- All function calls are virtual, which means that the most derived function is called, except when the contract is explicitly given.

- Advanced inheritance topics: http://solidity.readthedocs.io/en/latest/contracts.html#inheritance

- inheritance-example.sol

---

## Libraries:
- http://solidity.readthedocs.io/en/latest/contracts.html#libraries
