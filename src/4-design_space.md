
# **Chapter 4: Solution Design Space**

# **Intro**

In previous chapters, we suggested ways to increase the attack cost by forcing the attacker to lock a significant fraction of the amount under attack.

We will now discuss why these solutions are not satisfactory and suggest better alternatives, which although require substantial changes to the protocol.


## **Solution efficiency**

A solution to channel jamming is efficient if it costs little for the victims, while making the attack cost substantially higher.

Both in the context of attacking individual nodes or the entire network, there are two ways to consider a solution satisfactory:



1. Routing nodes suffering from the attack should be compensated in full liquidity interest rate (as valued by the routing nodes before the attack), paid by the attacker;
2. Alternatively, for those solutions which don’t compensate the suffering routing nodes, the attack cost could be just so high** (substantially** higher than the liquidity interest rate) that an attacker won’t resort to the attack.

Neither of the incremental solutions proposed in Chapter 3 achieves these objectives.. Now, let’s discuss other ways to achieve them.


## **Design directions**

The two solution options from the previous chapter could be re-formulated as follows:



* S1: Make lengthy hold of funds a prohibitively expensive attack;
* S2: Allow routing nodes to charge for lengthy hold of funds.

A whole separate design direction is network-level solutions, which don’t seem feasible to us at the moment, since it would either require substantial LN redesign.

We also don’t consider changing the structure of channels in this Chapter. We believe that the Chapter 3 overviewed (“JIT Transaction Stagging”) the best we can achieve in this direction.

We focus on _the duration of the hold of funds_, and not the time the payment takes, because focusing on the duration of an individual payment would just encourage an attacker to split jamming payments into many short-living ones.

We see two ways to enable S2:



* W1. Bonding the user to pay the fee at the payment initiating phase;
* W2. Incentivizing the user follow the rules (e.g., pay the fees afterwards) via a reward/punishment scheme.

Both W1 and W2 could be used to enable S1, if a corresponding routing node just sets the lengthy-hold fees prohibitively high.

**Node-level protection vs. network-level protection**

In the previous Chapters, we discussed how jamming could hurt individual nodes and LN as a whole. As for the defense protocols, it doesn’t make sense to distinguish the two.

We omit solutions which introduce a new coordination entity/protocol since they would result in significant centralization risks. The best we can do is enable routing nodes to protect themselves by giving them the right tool. Let’s overview some of them now.


### **LN culture: capital efficiency vs. financial access (DLC, Swaps, etc)**

There is a family of non-malicious LN activities, which require lengthy (hours to weeks) holding of funds: [DLC](https://github.com/discreetlogcontracts/dlcspecs/), [swaps](https://github.com/ElementsProject/peerswap), etc. We will call them _LHF_ (lengthy holding of funds).

Currently, the LN protocol is oblivious to LHF. Routing nodes can’t:



* distinguish them from regular fast payments;
* charge them based on the lock period;
* cancel them if they hold funds for too long.

We believe that the LN protocol will have to take them into consideration. Otherwise, the emerging LHFs not paying higher fees would force routing nodes to charge higher fees both from them and from small payments.

We think that handling LHFs is equivalent to solving the jamming issue: if the ecosystem decides that LHFs are undesired, they could seek solutions among S1 or S2. Otherwise, they should seek it among S2.

Now, we will overview more concrete design directions. We will cover both W1 and W2, remaining oblivious of the S1/S2 choice.


## **W1: bonding to a fee payment**

Obliging a payment sender to pay for the lengthy payments could be done via:



* W1.1 a bond over the same payment channels
* W1.2 an on-chain Bitcoin smart contract (either pure or via oracles/third-parties)
* W1.3 third-party arbitrage
* W1.4 a smart contract on another blockchain

We believe that W1.1 should be prioritized and thoroughly explored, before proceeding to the latter ones, as the Lightning-only model makes the most sense in terms of threat model (better than W1.3 and W1.4) and convenience/efficiency (better than W1.2 and W1.4).


### **W1.1 Bonding the payer via the same payment channels**

There are **two ways to bond** a payer to the fee obligations via the same channels:



* directly (guaranteeing the fees will be paid to every routing node);
* transitively (guaranteeing to pay to the first hop, which pays for the second hop, etc.).

We believe that **the latter** direction falls into the existing LN operation model much **better,** and implementing the former would require substantial changes to the LN operation (e.g., leaking payment privacy to every routing node).

Another aspect is the **exact mechanism guaranteeing the fees are paid**:



* either game-theoretical (everyone is incentivized to resolve the payment faster);
* or contract-based (e.g., pre-signed transactions unlocking more funds towards the routing node while time passes; or additional HTLCs dedicated at every hop).

The fee-based game-theoretical design space stemmed from the evolution of a naive upfront payment proposal summarized [here](https://github.com/t-bast/lightning-docs/blob/master/spam-prevention.md#proposals), resulting in a _hold-time-dependent bidirectional upfront payment schemes_ were proposed. We overview them and put them in the Chapter 5.

_The contract-based design space is currently not well-explored, with a small exception for [hashcash-based schemes](https://lists.linuxfoundation.org/pipermail/lightning-dev/2019-November/002275.html)._


## **W2: Incentivizing the rules via rewards/punishments**

The most important part in designing a reward/punishment scheme is choosing the corresponding **resource. **In this case, **a collateral** and **the right to route payments** are two options that make most sense.

_We leave finding a trust-minimized scheme for collateral burning for future work, and focus on the latter option instead._

Among the latter solutions, the mechanism of **distributing those resources** should be decided (e.g., they could be purchased). Purchasing, however, is equivalent to the straightforward upfront payment schemes, which [were deemed to be flawed](https://github.com/t-bast/lightning-docs/blob/master/spam-prevention.md#naive-upfront-payment). Instead, we suggest assigning these rights based on the previous LN activity (e.g., previous payments).

This is effectively a reputation scheme consisting of **two components**:



1. An identity (e.g., a dedicated private/public keypair, a proof of UTXO ownership, a PoW token, etc.)
2. A reputation algorithm (how much is allocated initially; could it go to 0 and get the user banned; etc.)

_While implementing this policy, the following trade-off should be considered: making it harder to obtain a reputation hurts both an attacker and honest user. We believe that a good reputation algorithm is a key answer to this trade-off._

An identity **could be anonymized** by proving ownership without revealing the item (e.g., a zero-knowledge proof of owning a UTXO). This works only for those identities, where a public list is available (not for PoW tokens).

A payer’s reputation could be **either localized by every routing node or** **shared** **among many routing** **nodes** (possibly, along with the proofs of malicious behavior).

We believe that the latter could hurt the payer’s anonymity, and requires a serious reliable infrastructure, which becomes an attack surface (both exploit-wise and regulatory-wise).

Now, let’s **focus on the former**.


### **W2.1 Local reputation**

There are two fundamentally different **ways to implement a locally-enforced reputation** used to decide whether to forward payment or not: **direct** (looking at the payer) **or transitive** (looking at the previous hop, which would look at the previous hop, etc.).

_We believe that trivial versions of the latter are prone to the attacker manipulations of the reputation between two honest nodes. We leave a thorough exploration of this direction for further research. For now, we focus on the former family of solutions._

Currently, routing nodes don't know who initiated the payment. Within a reputation protocol, every payment could be **associated with an identity**, **either within the onion, or out-of-band**.

**Proving reputation ownership could be non-interactive** (via one round of communication, a payer sends a proof to a routing node) **or interactive** (three rounds, a payer asks a routing node for a challenge, and then submits an associated proof). The latter makes selling the proofs on the secondary market harder but requires a more sophisticated implementation.

Reputation schemes may use “secondary assets” (e.g., reputation tokens) or just stick to the original identity proof.

Another important component of the reputation system is the **reputation formula.** Since it could be different at every routing node, it could be either **derivable/asked by the payers** at every routing node, or **oblivious to payers**.

In one of the following Chapters, we will discuss concrete reputation protocols in more detail.


## **Conclusions**

We attempted to overview the design space of fundamental solutions to channel jamming. We believe this helps in better understanding all the alternatives, and making a better informed decision in the end.

Within this design space, we highlighted two most promising directions:



* bonding the payer to pay the fee via the same payment channels;
* forwarding payments based on the locally-tracked reputation on the payment sender.

We also suggested the directions which are currently under-explored.

In the following Chapters, we will discuss implementations of these promising directions.
