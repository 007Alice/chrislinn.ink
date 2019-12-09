# Consensus Properties

<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

## ACID

## CAP

## SMR
- [State machine replication](https://dl.acm.org/citation.cfm?id=98167) is a general paradigm for implementing fault-tolerant services by replicating servers and coordinating client interactions with server replicas.
    + Place copies of the State Machine on multiple, independent servers.
    + Receive client requests, interpreted as Inputs to the State Machine.
    + Choose an ordering for the Inputs.
    + Execute Inputs in the chosen order on each server.
    + Respond to clients with the Output from the State Machine.
    + Monitor replicas for differences in State or Output.
- Properties
    + safety
        * (Informal) When decisions are made by any two correct nodes, they decide on non-conflicting transactions.
        * 诚实的节点对合法交易将达成统一的 (consistent) 意见
    + liveness
        * (Informal) \\(T\\)-Liveness: each honest node terminates and outputs a value at the end of \\(T\\).
            - The value of \\(T\\) depends on the research problems and protocols.
        * 一笔合法交易在合理时间长度内会被确认
    + total ordering
        * taken from [BEAT: Asynchronous BFT made practical (DRZ18)](https://www.csee.umbc.edu/~hbzhang/files/beat.pdf)
        > If a correct replica has delivered \\(m_1, m_2, \dots ,m_s\\) and another correct replica has delivered \\(m'\_1, m'\_2, \dots , m'\_{s'}\\), then \\(m_i = m'\_i\\) for \\(1 \leq i \leq min(s, s')\\).

## CFT

## BFT

## Consensus
* https://disco.ethz.ch/courses/podc_allstars/lecture/chapter16.pdf
> There are \\(n\\) nodes, of which at most \\(f\\) might be faulty (or Byzantine). Each node \\(P_i\\) starts with an input value (say \\(u_i\\)). The nodes must decide one of those values (say \\(v_i\\)), satisfying three properties: Agreement, Validity and Termination.
    + Classic
        * Agreement
            - All correct processes must agree on the same value.
            - For any two honest players \\(P_i\\) and \\(P_j\\), \\(v_i = v_j\\).
        * Validity
            - The decision value must be the input value of a node.
            - \\(\forall i: u_i \in \langle v_i \rangle\\).
        * Termination
            - (Informal) All correct nodes terminate in finite time.
            - All the honest players terminate with probability 1.
    + Emerging
        * Linearity
            - First proposed in [HotStuff](https://arxiv.org/abs/1803.05069).
                - PODC'19  分布式计算理论顶会
                - Facebook's LibraBFT
            - Any correct leader sends only \\(O(n)\\) messages to drive a protocol to consensus.
        * Responsiveness
            - First defined in [Hybrid Consensus](https://eprint.iacr.org/2016/917.pdf).
                - DISC'17 分布式计算理论顶会
            - (Informal) The transaction confirmation time depends only on the network’s actual delay, but not on any a-prior known upper-bound.
            - A consensus protocol is responsive if nodes can reach the consensus in time depending only on the network’s actual \\(\delta\\) (message delays), not on the loose upper bound \\(\Delta\\) (known upper bound on message delays).

## Blockchain
- Common prefix (Consistency)
    + \\(k\\)-common-preifx
        * First proposed in [The bitcoin backbone protocol: Analysis and applications (GKL15)](https://eprint.iacr.org/2014/765.pdf).
            - For any pair of honest players \\(P_1\\), \\(P_2\\) adopting the chains \\(C_1\\), \\(C_2\\) at rounds \\(r_1 \leq r_2\\), it holds that \\(\mathcal{C}_{1}^{\lceil k} \preceq \mathcal{C}_2\\).
    + \\(T\\)-consistency
        * [Analysis of the blockchain protocol in asynchronous networks (PSS17)](https://eprint.iacr.org/2016/454.pdf) refines Common Prefix to \\(T\\)-Consistency in order to provide a black-box reduction.
* Chain growth
    + \\((\tau, s)\\)-Chain growth
        * For any honest party \\(P\\) with chain \\(C\\), it holds that for any  \\(s\\) rounds there are at least \\(\tau \cdot s\\) blocks added to the chain of \\(P\\).
- Chain quality (Fairness)
    + \\((\mu, k)\\)-Chain quality (Fairness)
        * The proportion of blocks in any \\(k\\)-long subsequence produced by the adversary is less than \\(\mu \cdot k\\), where \\(\mu\\) is the portion of mining power controlled by the adversary.