# modular exponentiation when the modulus is secret shared

1. [Multiparty Generation of an RSA Modulus](https://eprint.iacr.org/2020/370.pdf)
    +  Omer:
        +  > This paper, written by some of my favourite authors, is introducing a new trick - instead of rejection sampling - they use CRT in a brilliant way. They keep using CRT throughout other parts of the protocol which makes for substantial improvement in efficiency over FLOP18. Another nice thing is that they use UC for the first time and it turns out to be very elegant and modular as it should be. For fans of the genre: along the way they deal with some subtleties in the simulators which makes the read even more fun. Highly recommended
2. [Diogenes: Lightweight Scalable RSA Modulus Generation with a Dishonest Majority](https://eprint.iacr.org/2020/374.pdf)
    + Multiparty RSA modulus generation that scales to thousands of parties and tolerate dishonest majority.
        * the best kind of security (n-1 active security with identifiable abort)
    + VDF alliance uses this
    + Omer:
        + > This highly anticipated paper, presents a full system, covering ALL the missing pieces to allow the protocol to run at scale. They use the CRT trick (its one of the things that once you see it, cannot be unseen), they replace the OT used in previous two papers with homomorphic encryption based approach, they claim to achieve identifiable abort, they use Ligero zero knowledge proof system for the malicious security (which means one basically put everything in a statement and prove it) and they introduce a new role of a Coordinator to help facilitate communication. They try to follow the line of UC security as well. This paper is crazy. Oh and It includes benchmarks of up to 4000 parties.


Runchao:

> But there is one concern. While being able to scale to thousands of parties, Diogenes seems to assume private broadcast channels, and work under synchronous networks. Is it realistic that a node hash thousands of peers and such large-scale network can remain synchronous?

> Otherwise use this protocol in real world, there should be a TTP for relaying messages, and this TTP should not be able to learn anything about the generated modulus. This seems to be achievable