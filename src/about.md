# About

This website was created to summarize the latest state of solving Channel Jamming issue of the Lightning Network.

Here, we talk about the costs and impacts of jamming. We also explore the solution space, and discuss some concrete ideas: Stake Certificates and Forwarding Pass.

_We assume some basic knowledge of channel jamming. Some general background can be found here: **[Preventing Channel Jamming](https://blog.bitmex.com/preventing-channel-jamming/).**_

Our goal was to align understanding of the problem and potential solutions across LN ecosystem actors, so that moving further becomes more productive.

You might find the content interesting while discussing about solving other spam issues (in LN or elsewhere), or stuff related to deploying DLC (and other "lengthy hold of funds" type protocols) in the LN.

Almost every chapter mentions open research problems, which could hopefully help someone to find a cool topic to work on.

## Authors

The initial version was deployed by Antoine Riard (`antoine@thelab31.xyz`) and Gleb Naumenko (`gleb@thelab31.xyz`). Feel free to reach out.

This content was sponsored by [NYDIG](https://nydig.com/) and [ACINQ](https://acinq.co/).

## Resources

In our work, we relied on the prior research:
*  [t-bast/spam-prevention.md](https://github.com/t-bast/lightning-docs/blob/master/spam-prevention.md)
* [Discharged Payment Channels: Quantifying the Lightning Network’s Resilience to Topology-Based Attacks](https://arxiv.org/pdf/1904.10253.pdf)
* [LockDown: Balance Availability Attack Against Lightning Network Channels](https://eprint.iacr.org/2019/1149.pdf)
* [Congestion Attacks in Payment Channel Networks](https://arxiv.org/pdf/2002.06564.pdf)
* [Probing Channel Balances in the Lightning Network](https://arxiv.org/pdf/2004.00333.pdf)
* [A Quantitative Analysis of Security, Anonymity and Scalability for the Lightning Network](https://eprint.iacr.org/2020/303)
* [Analysis and Probing of Parallel Channels in the Lightning Network](https://eprint.iacr.org/2021/384)

For some of the experiments, we [forked](https://github.com/naumenkogs/ln-probing-simulator/commit/a4660b5fc7d1c7145b0b91cb7d6361193eeb6f7a) the ln-probing-simulator [repo](https://github.com/s-tikhomirov/ln-probing-simulator).

## Contributing

We intend to update this website following open-source practices through the [GitHub repo](https://github.com/naumenkogs/jamming-dev.github.io).

## License

This website is released under the terms of the [MIT license](https://opensource.org/licenses/MIT).