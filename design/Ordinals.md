# Ordinals
[ord](https://github.com/ordinals/ord)  
[ordinalswallet](https://ordinalswallet.com/)    
[rune](https://github.com/ordinals-wallet/rune)     
[ordinals-collections](https://github.com/ordinals-wallet/ordinals-collections)  

[runes](https://ordinals.com/runes)  
[ordinals doc](https://docs.ordinals.com/)  
[ordinals](https://github.com/ordinals)   
[runebtc](https://www.runebtc.xyz/)     
[bitcoinrune doc](https://docs.bitcoinrune.xyz/)         
[signet ordinals](https://signet.ordinals.com/)  




# 1.Ordinal Theory Overview

## Ordinal numbers have a few different representations:

* Integer notation: 2099994106992659 The ordinal number, assigned according to the order in which the satoshi was mined.
* Decimal notation: 3891094.16797 The first number is the block height in which the satoshi was mined, the second the offset of the satoshi within the block.
* Degree notation: 3°111094′214″16797‴. We'll get to that in a moment.
* Percentile notation: 99.99971949060254% . The satoshi's position in Bitcoin's supply, expressed as a percentage.
Name: satoshi. An encoding of the ordinal number using the characters a through z.


Bitcoin has periodic events, some frequent, some more uncommon, and these naturally lend themselves to a system of rarity. These periodic events are:

* Blocks: A new block is mined approximately every 10 minutes, from now until the end of time.

* Difficulty adjustments: Every 2016 blocks, or approximately every two weeks, the Bitcoin network responds to changes in hashrate by adjusting the difficulty target which blocks must meet in order to be accepted.

* Halvings: Every 210,000 blocks, or roughly every four years, the amount of new sats created in every block is cut in half.

* Cycles: Every six halvings, something magical happens: the halving and the difficulty adjustment coincide. This is called a conjunction, and the time period between conjunctions a cycle. A conjunction occurs roughly every 24 years. The first conjunction should happen sometime in 2032.

This gives us the following rarity levels:

* common: Any sat that is not the first sat of its block
* uncommon: The first sat of each block
* rare: The first sat of each difficulty adjustment period
* epic: The first sat of each halving epoch
* legendary: The first sat of each cycle
* mythic: The first sat of the genesis block


# 3.Inscriptions

Inscription content is entirely on-chain, stored in taproot script-path spend scripts. Taproot scripts have very few restrictions on their content, and additionally receive the witness discount, making inscription content storage relatively economical.

Since taproot script spends can only be made from existing taproot outputs, inscriptions are made using a two-phase commit/reveal procedure. First, in the commit transaction, a taproot output committing to a script containing the inscription content is created. Second, in the reveal transaction, the output created by the commit transaction is spent, revealing the inscription content on-chain.

Inscription content is serialized using data pushes within unexecuted conditionals, called "envelopes". Envelopes consist of an OP_FALSE OP_IF … OP_ENDIF wrapping any number of data pushes. Because envelopes are effectively no-ops, they do not change the semantics of the script in which they are included, and can be combined with any other locking script.

A text inscription containing the string "Hello, world!" is serialized as follows:

```
OP_FALSE
OP_IF
  OP_PUSH "ord"
  OP_PUSH 1
  OP_PUSH "text/plain;charset=utf-8"
  OP_PUSH 0
  OP_PUSH "Hello, world!"
OP_ENDIF
```


3.1. Delegate
3.2. Metadata
3.3. Pointer
3.4. Provenance：收藏品 collection
3.5. Recursion：递归
3.6. Rendering

```
OP_PUSH 0
OP_PUSH 1
OP_PUSH 2
OP_PUSH 3
OP_PUSH 5
OP_PUSH 11
```

# 4.Runes

## Runes Does Not Have a Specification
## Runestones

//发行
```
struct Etching {
  divisibility: Option<u8>,
  premine: Option<u128>,
  rune: Option<Rune>,
  spacers: Option<u32>,
  symbol: Option<char>,
  terms: Option<Terms>,
}
```

```
struct Terms {
  amount: Option<u128>,
  cap: Option<u128>,
  height: (Option<u64>, Option<u64>),
  offset: (Option<u64>, Option<u64>),
}

```

```
struct Rune(u128);
```

//认购
```
struct Runestone {
  edicts: Vec<Edict>,//法令
  etching: Option<Etching>,//刻蚀
  mint: Option<RuneId>,
  pointer: Option<u32>,
}
```

```
struct Edict {
  id: RuneId,
  amount: u128,
  output: u32,
}

```

```
struct RuneId {
  block: u64,
  tx: u32,
}

```




## Deciphering:解读

Deciphering
Runestones are deciphered from transactions with the following steps:

1. Find the first transaction output whose script pubkey begins with OP_RETURN OP_13.
2. Concatenate all following data pushes into a payload buffer.
3. Decode a sequence 128-bit LEB128 integers from the payload buffer.
4. Parse the sequence of integers into an untyped message.
5. Parse the untyped message into a runestone.




# 8. Guides
8.1. Explorer
8.2. Wallet
8.3. Batch Inscribing
8.4. Collecting
8.4.1. Sparrow Wallet
8.5. Moderation
8.6. Reindexing
8.7. Sat Hunting
8.8. Settings
8.9. Teleburning
## 8.10. Testing

Test Networks
Ord can be tested using the following flags to specify the test network. For more information on running Bitcoin Core for testing, see Bitcoin's developer documentation.

Most ord commands in wallet and explorer can be run with the following network flags:

Network	Flag

```
Testnet	--testnet or -t
Signet	--signet or -s
Regtest	--regtest or -r
```

**Regtest doesn't require downloading the blockchain since you create your own private blockchain, so indexing ord is almost instantaneous.**

Example
Run bitcoind in regtest with:
```
bitcoind -regtest -txindex
```
Run ord server in regtest with:
```
ord --regtest server
```
Create a wallet in regtest with:
```
ord --regtest wallet create
```
Get a regtest receive address with:
```
ord --regtest wallet receive
```


```
```