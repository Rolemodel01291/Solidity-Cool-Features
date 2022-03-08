# Solidity-Cool-Features
Here in this page, I am introducing a couple of tips that allows Soldiity developers to optimize their code.

Smart contract developers often feel gas optimization is painful, particularly in complex smart contracts. Hence, it is crucial to leverage some techniques to optimize smart contract source code rather than bytecode. In this short article, I will introduce some of these techniques for Solidity smart contracts, so happy optimization!



### Custom Errors in Solidity 0.8
Defining custom errors and using them with Revert, instead of String reduces the gas consumption in the Solidity smart contracts.
What is it like that?
If you use a string to show the error, the amount of the gas depends on the string length. For example:


```
pragma solidity ^0.8.4;

contract Playground {
    address payable owner = payable(msg.sender);

    function withdraw() public {
        if (msg.sender != owner)
            revert(" This is a sample error string! You should not use it because it's costly!");

        owner.transfer(address(this).balance);
    }

}
````
 The execution cost of the withdraw function now is ***23678*** gas; the longer the error message is, the more gas is going to consume.
 
 #### So now what?
 Now we want to take advantage of new features introduced in Solidity 8 in order to reduce our gas consumption!
 
 ```
pragma solidity ^0.8.4;

error Unauthorized (address caller);  // here is our custom error
contract Playground {
    address payable owner = payable(msg.sender);
    

    function withdraw() public {
        if (msg.sender != owner)
            revert Unauthorized(msg.sender);

        owner.transfer(address(this).balance);
    }
    // ...
}
 
 ````
  The execution cost of the withdraw function now is ***23591*** gas!
  
  
  address public deployedAddr;

    function createDSalted(bytes32 salt, uint arg) public {
        D d = new D{salt: salt}(arg);
        deployedAddr = address(d);
    }
 

#### Caching Local State Variables
If you are familiar with databases in software engineering, you know that fetching data from databases are slower than fetching data from memory.
We have the same concept in Solidity as well, but here we can exchange the term "Database" with "Local State" variables.

Take a look at the following example with high gas consumption before the cashing technique:

````
pragma solidity ^0.8.4;

contract Test{

    unint public n=5;

    function nonoptimized() external view returns (unint) {

    uint a=0;
        for(uinit i=0; i<n; i++){ // in each iteration, we have to check the local state variable n, which is a slow approach
          a+=1;
         }
      return a;
    }
    
   function optimized() external view returns (unint) {

     uint a=0;
     unit cach_n = n; // here we cach the local state variable into a local variable that does not consume expensive memory
        for(uinit i=0; i<cach_n; i++){ // in each iteration, we have to check the local state variable n, which is a slow approach
          a+=1;
         }
      return a;
    }
    
}

```
"# Solidity-Cool-Features" 
