#Ethereum provides different testnets for testing. We will use rinkeby testnet.

Following instructions from the excellent https://www.rinkeby.io/

It is highly recommended to synchronize a Full Node.
A full node lets you access all state. There is a light node (state-on-demand) and wallet-only (no state) instructions as well, and these are even faster.

From the docs above:

A full node synchronizes the blockchain by downloading the full chain from the genesis block to the current head block,
but does not execute the transactions. Instead, it downloads all the transactions receipts along with the entire recent state.
As the node downloads the recent state directly, historical data can only be queried from that block onward.

Initial processing required to synchronize is more bandwidth intensive, but is light on the CPU and has significantly reduced
disk requirements. Mid range machines with HDD storage, decent CPUs and 4GB+ RAM should be enough.
Step 1: Download Geth
First, install the latest geth (1.7.3) to your machine.

For Ubuntu, you can follow the instructions on the official wiki.

Option 1:
```sh
sudo apt-get install software-properties-common
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum
If you're just upgrade geth from a previous version, you can just run

sudo apt install geth
```
Option 2:

Use source code and build it locally.
You can clone it using following command
```sh
git clone https://github.com/ethereum/go-ethereum.git
```
and follow the instructions in README.md to build it.

Step 2: Run Geth in Rinkeby Mode
At this point, you should probably start a tmux or screen session, so if you get interrupted during syncing it will still keep going in the background.

To run a full node, start Geth with the Rinkeby switch:
```sh
geth --rinkeby
```
SECURITY WARNINGS: We've enabled RPC and also loaded the personal module to allow testing and participating in smart contracts. However, if you do these things on a mainnet node with your unlocked wallet exposed to the internet, you could get hacked and all your monies stolen. I'll write a separate gist about a secure way to participate in a mainnet contract with real ETH.

It took quite a long time(~2 hours) for me to sync the fullnode as I am running linux in VM.

Step 3: Create an account

Open new terminal window and after that, attach the console with the appropriate data directory.

On Linux it would be:
```sh
geth --datadir=$HOME/.ethereum/rinkeby attach ipc:$HOME/.ethereum/rinkeby/geth.ipc console
```
On Mac it would be:

```sh
geth --datadir=$HOME/.ethereum/rinkeby attach ipc:$HOME/Library/Ethereum/rinkeby/geth.ipc console
```
and create an account (substituting a much better password than "secretpassword").
```sh
Welcome to the Geth JavaScript console!

instance: Geth/v1.8.0-unstable-40656953/linux-amd64/go1.8.3
coinbase: 0xa0ac7d666288299a9588ffc0ed0d6c6ab1fbee2d
at block: 1760333 (Mon, 12 Feb 2018 14:36:11 PST)
 datadir: /home/ankur/.ethereum/rinkeby
 modules: admin:1.0 clique:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
>

> eth.accounts
[]
> personal.newAccount("typeyourpassword")
"0xa0ac7d666288299a9588ffc0ed0d6c6ab1fbee2d"
> eth.coinbase
"0xa0ac7d666288299a9588ffc0ed0d6c6ab1fbee2d"
> eth.getBalance(eth.coinbase)
0
```
You'll see a different address than 0xa0ac7d666288299a9588ffc0ed0d6c6ab1fbee2d. That one's mine, provided for illustration. Save your password in a secret place, preferrably encrypted. I use Evernote encrypted text, but you can use any password manager like 1Password, LastPass, Dashlane, etc.

Leave that terminal open for now.

Step 4: Request ETH
Because Kovan and Rinkeby both use Proof-of-Authority (clique) to grant ETH, you'll need to request some to get started. However, unlike Kovan which requires you to bootstrap by requesting KETH from another human being, Rinkeby has a super-slick automated faucet, where you submit your address (copied from above) into one of three methods:

A public tweet on Twitter
A public Facebook post
A public Google+ link
Since I never use Google+ for social reasons with humans, I might as well use this for socializing with robots.

You can go to http://plus.google.com and post publicly on any discussion board. Here's mine.

Copy this URL:

https://plus.google.com/+AnkurSatasiya/posts/biTbfeJJu2Q
Go to the https://www.rinkeby.io/#faucet and paste it into the blank.

Choose an option from the dropdown which corresponds to how much Ether you need and how frequently (requesting more Ether will take longer between requests). I requested 7.5 ETH in 1 day and I got it immediately. Don't worry, you'll get your ETH in seconds, but you can't request again for another 24 hours. This is to prevent spammers from swamping the network by overpowering it with mining power and then out-spending everyone else.

This is the transaction where I received my 7.5 ETH: https://rinkeby.etherscan.io/tx/0xcbfe219a90cfa9d9e2611b2c124c6cdd5f62319192ad961a3d204a02955cb8bd

Now, back in your geth console, wait for at most 15 seconds for the next block to be found, and verify your balance again
```sh
> eth.getBalance(eth.coinbase)
75000000000000000000
```
Woohoo! You're rich, in testnet wei :)

You can also retrive the transaction details.
```sh
> eth.getTransaction("0xcbfe219a90cfa9d9e2611b2c124c6cdd5f62319192ad961a3d204a02955cb8bd")
{
  blockHash: "0x9d467f3e23c02e27504f1e2f721464eddd683192715626d7f532b1cea7419f68",
  blockNumber: 1754089,
  from: "0x31b98d14007bdee637298086988a0bbd31184523",
  gas: 21000,
  gasPrice: 1000005109,
  hash: "0xcbfe219a90cfa9d9e2611b2c124c6cdd5f62319192ad961a3d204a02955cb8bd",
  input: "0x",
  nonce: 56319,
  r: "0x853cb976f5b35252b4b4575147b8a19bee2545bbeafa5a97769d5903f3eecafb",
  s: "0x6f1c3f59fe12b2c441f0d402e0dc37ad6427eb8f5f52235db4be7908fc2a3d9",
  to: "0xa0ac7d666288299a9588ffc0ed0d6c6ab1fbee2d",
  transactionIndex: 7,
  v: "0x2c",
  value: 7500000000000000000
}
```
Note: Before you run all this commands make sure that the following command returns false.
```sh
> eth.syncing
false
> 
```
If your note will be syncing then it will only show details of the transactions which are synced.

You can also leave questions, comments, or feedbacks on this gist.
