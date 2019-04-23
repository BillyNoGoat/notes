#Udemy blockchain course

##Blockchain
A blockchain is a constantly growing ledger that keeps a permanent record of all the transactions that have taken place in a secure, chornological and immutable way.\
A blockchain is a ledger/file which keeps track of all transactions in a permanent way. It's placed in a secure way using advanced cryptography and it's done chronologically.
Immutable means it can't ever be changed.

##Miners
- Miners could be anyone but it's generally done by dedicated powerful hardware in dedicated conditions
- Every transaction needs to be cryptographically encoded and secured. This ensures nobody tampers with data
- Miners get paid in bitcoin
- The only way bitcoin is created is via **mining**
- They basically build the blockchain. New blocks are added every 10 minutes with all the new transactions. They build a block and when a block is confirmed it's added to the blockchain.
- The first ever block is called the genesis block

##Cryptography vs hash
A cryptographic hash is a digest/digital fingerprint of a certain amount of data. This is a FIXED LENGTH. Bitcoin uses SHA256 to hash.\
SHA stands for secure hash algorithm and this was created by the NSA (National security agency).\
You can ALWAYS generate a hash from a certain amount of data and the output will always be the same if the input is the same.\

Hashes are **one way** functions. There's no way to go back to the original text from your output hash. This is DIFFERENT to encryption\.
With a hash you can never go back to the original text from your hash/fingerprint/digest whereas with encryption, using the correct key you could get the original data back by de-crypting the data.\

##Blocks in a blockchain
We can use [this tool on anders.com](https://anders.com/blockchain/block.html) to look at an example of a block.\
What we can see is that a block is of the following:
- Block number
- Nonce (Number used once)
- Data
- Cryptographic hash

####Breakdown of a block
#####0000 hash prefix
Here we can see that the hash begins with `0000` which will come in handy as it describes when a block is valid or not.\
You will notice that if we make a change like a single character in the data field it will get a completely different hash which doesn't begin with `0000` meaning it isn't considered a valid block.\
For this reason we will need to figure out how to make this a valid block which is where we will use **nonce**.

#####Nonce
Nonce is short for "Number used once" which is a random number used for figuring out how you can make that specific block give you a valid block (In this case that's by having 0000 at the beginning).\
If we change the value of the Nonce you will notice the cryptographic hash does not begin with 0000 and therefore isn't considered valid. We can then use the "mine" button to check what the correct **nonce** is for *ONLY* this block number.

##Blockchain explanation
We can use [this other tool on anders.com](https://anders.com/blockchain/blockchain.html) to demonstrate the chronological order of a blockchain. The blocks in this blockchain contain all the information shown previously in a block but with the addition of a new field which is the `prev` (previous) field which contains the hash of the block which came previous to this one.

As you can see, every block in the blockchain is cryptographically tied to the previous one from the cryptographic hash. This means that if you were to change any data in the blockchain, it would invalidate all blocks in front of it due to the hash being changed and all future blocks would need their `previous` hashes changed. This is demonstrated in the tool when changing any data with blocks proceeding it.

If we were to change a block in the middle of the blockchain, we would need to mine every block after that to get the correct Nonce value to ensure the block begins with `0000`.

####The importance of 0000 prefix on a hash
The `0000` at the beginning is tied to the difficulty in the blockchain which determines how difficult it is to get the equivalent cryptographic hash for a block. In our case, we need to have a hash which has at least 4 0's at the beginning.\
The difficulty on the blockchain will adjust every 2 weeks as more cryptographic hashing power is used on the network. More hashing power = difficulty needs to go up to ensure computers solving blocks will mine about one block every 10 minutes collectively.

So when a block is being mined, all computers mining on the network are trying to race to find out the required nonce to come up with a cryptographic hash which will be LESS than the required target for the difficulty level. If anybody were to try and tamper with a block, you would not only break that block but you would break every other block in that chain and have to re-mine every block afterwards.

###Bitcoin
Every 10 minutes, the software for bitcoin issues a challenge to find the nonce which will make the hash for a specific block be a valid hash which requires a specific number of zeroes. This is done by the the miners.

When the challenge is issues, there's a race that goes off which is all miners competing against each other to find the solution for the nonce which will satisfy that hash. It's just a guessing game of testing millions and billions of combinations to try and find that exact nonce which will satisfy that block. At a point, one of those miners will get the reward for solving that block. Every other miner in the network then has to verify the validity of that block.

It's a competition and a co-operation. The co-operation is mining and the verification step is called `Proof of work`. If the proof of work (Which is proving you were actually able to find the nonce) then the block is considered valid and is added to the bitcoin blockchain. Each block will include all the transactions that miner has said is valid.

That miner will then earn a reward. The reward number goes down over time but is currently at 12.5 bitcoins.

####Block size
The bitcoin block size is capped at 1 mb per block which is a huge limitation for bitcoin because it means only the top paying 1mb of transactions will be transacted which is why there's lots of support to increase the size per block.

####Total bitcoins
Bitcoin is hard capped to reach 21 million total bitcoins before it stops rewarding miners for bitcoins. This increases by 12.5 bitcoins every 10 minutes due to what we mentioned earlier with the reward bitcoins.
