# Fair contract

First priewienv's [_secret sharing_](https://blog.priewienv.me/post/randomness-blockchain-1/) draws my attentions.

Some nice papers:

+ [Timed Commitments](https://www.iacr.org/archive/crypto2000/18800237/18800237.pdf)
+ [Homomorphic Time-Lock](https://eprint.iacr.org/2019/635)
+ [Anonymous Multi-Hop Locks for Blockchain Scalability and Interoperability](https://www.ndss-symposium.org/wp-content/uploads/2019/02/ndss2019_09-4_Malavolta_paper.pdf)
+ [Concurrency and Privacy with Payment-Channel Networks](https://eprint.iacr.org/2017/820)
    * using Multihop HTLC

## Timed Commitments
+ the receiver is kinda guaranteed (I mean, with high probability) to recover the signature from the commitment after given time
    + makes use of time needed for computing squarings
        + gradual revealing, eliminating possibility by trying again and again 
    + high computing power won't speed it up
    + the committer also need to convince the receiver the commiment is indeed the commitment of the desired signature
        + use zkp (a simulator can produce...)
+ there also is a proof as the shortcut to verify, so that others don't need to go through the recovery process again to verify
+ not sure whether the use of CA will introduce problem?
+ applications
    * Contract Signing
    * Collective coin-flipping
    * Honest-Preserving Autions
        - vs [uncheatable auctions](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.24.6692&rep=rep1&type=pdf)
    * in Zero-Knowledge
        - by using force-opening

## Homomorphic Time-Lock
_TODO_