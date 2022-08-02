# Homework5

### 1. Look at the example of init code in today's notes ? https://gist.github.com/extropyCoder/4243c0f90e6a6e97006a31f5b9265b94
### When we do the CODECOPY operation, what are we overwriting ?

There are 3 args for CODECOPY opcode. It reads certain length of code from a memory location & copies it to the specified memory location. 

The deploy code uses CODECOPY to copy from transaction's input data to EVM memory. Means we are writing constructor arguments to the EVM memory with this opcode. 

### 2. Could the answer to Q1 allow an optimisation ?

MSTORE could be used instead of CODECOPY. MSTORE uses 3 gas. While CODECOPY's gas depends upon length to be copied. 

### 3. Can you trigger a revert in the init code in Remix ?

Yes a revert can be triggered in the init code, if there is some condition in the constructor that makes the contract deployment revert, or when sending wei in constructor since its not payable. 

Also, the given code in the above gist has `ISZERO` opcode in it. With this opcode, we check if the value in the stack is zero or non-zero. If its zero then 1 is assigned otherwise 0. Next opcode is `JUMPI`, which is a conditional jump which gets executed if the second item in the stack is non-zero. If the first 2 items in the stack are 0, means we do not return anything. So the transaction is reverted if the contract creation transaction has non-zero value. 

### 4. Can you think of a situation where the opcode EXTCODECOPY is used ?
EXTCODECOPY is used to check & compare contract's bytecode. This opcode can be used for contract whitelisting, that only those contracts can interact. Although this opcode is expensive & new opcode EXTCODEHASH was introduced which whitelist contract whose bytecode's hash matches. 

