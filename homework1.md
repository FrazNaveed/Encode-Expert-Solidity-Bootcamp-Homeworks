# Homework1

### 1. What information is held for an Ethereum account ?
The Ethereum account holds 4 things in it:
<ol>
<li>nonce: It is the number of transactions that are sent from this account. It gets incremented by 1 after a transaction. </li>
<li>balance: It is the amount of wei the account holds</li>
<li>storageRoot: Every Ethereum account has its own storage trie. This storage holds all the data
of a contract. This data is stored in a merkle patricia trie having key value pair (keccak256 => RLP encoding).
The hash of this storage trie is stored in the storageRoot. 
</li>
<li> codeHash: Hash refers to the code of an Ethereum account on EVM. If this address receives a message call, then 
this code is executed. </li>
</ol>

### 2. Where is the full Ethereum state held ?
There is only one state trie that holds the Ethereum state. The state trie holds the storage trie. The hash of which it is stored in state Root of block. 

### 3. What is a replay attack ? which 2 pieces of information can prevent it ?
Replay attack is repeating the same valid trasmission twice. Means taking a transaction from one blockchain & repeating it on other. To prevent replay attack chainId was added to the transaction signature & by adding nonce. 

### 4. What checks are made on transactions for view functions ?
While calling the view function the visibility of the function is checked. 


# Additional Notes: 
- Ethash is the modified form of dagger-hashimoto which is the Proof of Work algorithm for Ethereum. 
- A fork happens when there is division between the network about the best chain. 


### Mining in Etheruem: 
In Bitcoin, there were specialized ASIC chips which were used for mining. 
In Etheruem this scenario was changed. They wanted mining to be done on standard hardware like Laptops, PCs i.e hardware having large memory. These memory holds large datasets. This large dataset has to exist in memory 
to produce a random number. We need to grab the random slices of this dataset & hash them. That hash is the nonce for which the miners compete. 

### Block Validation in Ethereum: 
There is a race to solve that particular hash value. If someone solves that hash, then it is allowed to produce the block. After the miner produeces the block, it is propagated throughout the network. Then the network has the
opportunity to accept that & add to their local view of the blockchain. Here comes the consensus part. 

### GHOST Protocol (Greediest Heaviest Obeserved Subtree): 
Some blockchains have very low time to add the block. Like Ethereum block is added in about 15 seconds, while in Bitcoin a block takes 10 minutes to add in the network. 
When a miner solves the nonce & while it is being propagated in the network, there is possibility someone is still mining & founds the solution to the nonce. In Bitcoin they are called orphans & in Ethereum they are called uncles/ommers & they are still added in the blockchain in the next 7 blocks unlike Bitcoin. The effort of that miner isn't wasted and is still awarded some reward. In Bitcoin the longest chain is followed but in Ethereum the chain with the most uncles is followed. 

### Bloom Filters: 
Bloom filters is a probabilistic, space efficient data structure used for fast checks of set membership. 
If we want to check an entry in large dataset then bloom filters are used. 
-> hash each new element that goes into the dataset
-> take certain bits from this hash & then use these bits to fill in parts of a fixed-sized bit array (e.g some bits are set to 1). 
    This bit array is called bloom filter. 

When we want to check: 
We simply hash the element, check that the right bits are 1 in the bloom filter. If any of the bit is 0 then the 
element is definitely not in the dataaset. If all are 1, then it might be there. 

In Ethereum logs are generated for events. Those logs are stored in bloom filters. Each event has a topic. That topic is used to search the certain event. We take a event hash & figure it out which bits to look at in the block-level bloom filter. If any of the bits are 0, then the event must not have been generated in any transaction of the block. 

When a block is generated or verified, the address of any logging contract, & all the indexed fields from the logs generated are added to bloom filter, which is included in the block header. The actual logs are not stored to save space. 
When an application wants some events, the node can quickly scan over the header of each block, checking bloom filter if it contains the relevant log. 