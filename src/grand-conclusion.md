# Subjective Grand Conclusion

Here are our (Antoine's and Gleb's) personal thoughts on the subject.

We believe that Channel Jamming is a substantial risk to the LN, both against individual nodes and the network as a whole.

A good solution to jamming should either:
- guarantee the routing nodes could be compensated in full for locking their liquidity;
- or, otherwise, locking their liquidity should cost substantially more than the harm done.

The former approach also makes lengthy hold of funds (DLC, Swap protocols, etc.) feasible in the LN.

After overviewing known solution directions we identify the following matrix of solutions:
- Fees could be not charged at all (every lengthy lock implies a ban), be charged upfront (pessimistic), or charged afterwards (optimistic);
- Reputation ID may be based on a plain keypair or on Stake Certificates;
- Forwarding Pass may or may not be applied to introduce routing tokens.

Since every aspect adds susbtantial complexity to the system, we should make sure that improvements they bring are worth it. More specifically,
we should evaluate:
- whether reputation-based approach could overcome the discussed asymmetries between an attacker and an honest user (for both SC and FP);
- whether FP could efficienlty balance the risks between payment sender/recipient and routing nodes;

