# solidity-utils
This library implements the missing usable and iterable dictionary in solidity.  
Current implementations:
 * dic(uint, bytes)
 * dic(bytes32, bytes)
 * dic(bytes32, uint)

## Dictionary
`mapping` + keys iteration.  
Inspired by `Dictionary` in python or 'Object` in javascript, `Dictionary` is an improved and faster mapping data-structure that allows you to retrieve all the keys in a mapping object while  minimizing storage usage.  
The data-stucture combines linked-list iteration style with solidity `mapping` hash-table.  

### Using
```sol
pragma solidity ^0.4.0;
// import the contract
import "github.com/RaphCayou/solidity-utils/contracts/lib/Dictionary.sol";

contract FooUint {
    // declare and use new Dictionary structure
    using Dictionary for Dictionary.Data;
    Dictionary.Data private dic;

    function Foo() public view returns (uint) {
        dic.set(1, "value");
        dic.set(2, "foo");
        // get an item
        dic.get(2); // => '0x666f6f' (byte hex of 'foo')
        // update an item's value
        dic.set(2, "value");
        // get all keys
        dic.keys(); // => [1, 2]
        // check if key exists
        var exists = dic.exists(1);
    }
}
contract FooBytes32 {
    // declare and use new Dictionary structure
    using DictionaryBytes32 for DictionaryBytes32.Data;
    DictionaryBytes32.Data private dic;

    function Foo() public view returns (uint) {
        dic.set(0xab12, "value");
        dic.set(0x45ba, "foo");
        // get an item
        dic.get(0x45ba); // => '0x666f6f' (byte hex of 'foo')
        // update an item's value
        dic.set(0x45ba, "value");
        // get all keys
        dic.keys(); // => [0xab12, 0x45ba]
        // check if key exists
        dic.exists(0xab12); // => true
    }
}
contract FooBytes32Uint {
    // declare and use new Dictionary structure
    using DictionaryBytes32Uint for DictionaryBytes32Uint.Data;
    DictionaryBytes32Uint.Data private dic;

    function Foo() public view returns (uint) {
        dic.set(0xab12, 1);
        dic.set(0x45ba, 2);
        // get an item
        dic.get(0x45ba); // => 2
        // update an item's value
        dic.set(0x45ba, 3);
        // get all keys
        dic.keys(); // => [0xab12, 0x45ba]
        // check if key exists
        dic.exists(0xab12); // => true
    }
}
```

### Basic Operations
`set(uint id, bytes data)`  
`get(uint id) --> bytes data`  
`remove(uint id)`  
`getSize() --> uint num of keys`  
`exists(uint id) --> bool id exists`  

`set(bytes32 id, bytes data)`  
`get(bytes32 id) --> bytes data`  
`remove(bytes32 id)`  
`getSize() --> uint num of keys`  
`exists(bytes32 id) --> bool id exists`  

`set(bytes32 id, uint data)`  
`get(bytes32 id) --> uint data`  
`remove(bytes32 id)`  
`getSize() --> uint num of keys`  
`exists(bytes32 id) --> bool id exists`  

### Iteration operations
Keys are stored based on the order you specify, allows you to iterate over keys.  
`keys()` - return an array of all the keys.  
`next(uint id)` / `prev(uint id)` - return the next/prev key for a given key.  
`firstNodeId` / `lastNodeId` - return the first/last key.  
Example iteration:
```solidity
uint nodeId = dic.firstNodeId;
while (nodeId != 0) {
  // do something
  nodeId = dic.next(nodeId);
}
```

### Insertion Options
`insertBeginning(uint id, bytes data)` - add a node to the begining of the list.  
`insertEnd(uint id, bytes data)` - add a node to the end of the list.  
`insertBefore(uint beforeId, uint id, bytes data)` - add a node before another.  
`insertEnd(uint beforeId, uint id, bytes data)` - add a node after another.  
The default order for `.set(uint id, bytes data)` is `insertEnd`.  

### Limitation
Key must be bigger than 0.  
