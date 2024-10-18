# The Flawed Economics of Solana

## Part One: The Validators

Solana is a blockchain ecosystem with a market capitalisation of US$67 billion at the time of writing. Solana consistently fluctuates between the fifth and sixth largest crypto ecosystems of all time. Solana was launched in 2020 as an alternative to Ethereum, offering smart contract and NFT solutions without the high fees or slow transaction waiting times. The native token that runs on the Solana blockchain is called SOL.

Solana uses a consensus mechanism called *Proof of Stake*, which it uses in conjunction with a system design technique called *Proof of History* (later re-developed to become *Proof of Replication*, but to avoid confusion, we will stick with the name *Proof of History* throughout the paper). The technical details of these mechanisms are unimportant for now, as this analysis focuses on the economic incentives of Solana validators, which are supposed to secure the entire network. The most important thing to know for now is that the *Proof of History* mechanism requires validators to vote, costing about $1.1$ SOL per day - *per validator* - at the time of writing.

Solana promises a high level of decentralisation. In this analysis I prove that it is not possible - without assuming consistent MEV revenues - to have a high level of validator decentralisation based on the current economic model. My results are backed anecdotally. Despite providing significant voting fee subsidies and technical assistance to new validators for years, in March 2024 Solana Labs voted to remove the burning of the 50% priority fee so that exisiting validators may have more economic incentive to secure the system[^1]. Based on the following analysis, this move will likely have little to no impact on the centralising nature of the Solana validator ecosystem. Incentivising decentralisation would require a complete overhaul of the *Proof of History* mechanism and its associated voting fees.

**Game Setting**

Validators have two key fixed annual costs $e$: voting fees $c_v$ and server fees $c_s$. One may assume that server fees for running a validator on the Cloud run between US$4500 to $6000 per year[^2], however we omit server fees in our analysis for brevity.

- $c_v$ - currently costs $1.95$ SOL per epoch, or around $1.1$ SOL per day.
- $EpY$ - Epochs per Year. The more transactions that occur on Solana, the more epochs there are. Currently there are around 168 epochs per year. We assume that this number is constant in the analysis.
-  $s_{i}$: total stake in SOL for validator $i$ 
- $Opp$ - Cost of stake, or else known as "opportunity cost". Validators often pay a yield to "stakers" that choose to stake with them[^3]. Assume for now $Opp = s_i * 0.05$ at a conservative yield of $5$%.

$$e_i = c_v* EpY + Opp $$

Validators earn revenue by the way of annual block rewards $r_{annual}$. The probability of getting a block reward $r_{block}$ for a validator $i$ is equal to the probability of being assigned the block leader, which is equal to the proportion of the validator's *stake* over the entire pool of staked SOL. 

$$
r_{annual} = \frac{s_i}{\sum_{i=1}^{i=n}s_i} * 432,000 * EpY * r_{block}
$$

- Solana has a fixed number of blocks (aka slots) per epoch: $432,000$.
- $r_{block}$ is the current block reward, which is determined by the base fee and prioritisation fees. For the analysis, we assume $r_{block}$ to be constant at $0.0156$ SOL, fluctuating marginally over time.

Validators can also earn revenue via a block ordering technique called **Maximum Extractable Value** (MEV). This technique is more controversial than not, since a lot of MEV is exploitative[^PWC] and revenues are volatile. For these reasons, MEV is not considered to be a foundational economic incentive for validators. 

**Analysis**

To find the minimum viable stake $\hat{s_i} = min{\frac{s_i}{\sum_{i=1}^{i=n}s_i}}$ a validator needs to have in order to ensure no losses, we derive the expected annual expense minus the expected annual reward for validator $i \in n$:

$$e_i - r_i = 0$$

Equating expense $e_i$ with the the reward $r$,

$$\hat{s_i}* 432000 * 0.0156 * 168 = e_i$$

Omitting $Opp$ for now, assume fixed expense $e$ for all $i$ at $e = c_v * EpY = 327.6$. 

$$1132185.6 *\hat{s_i}=327.6$$

Solving for $s_i$:

$$ \frac{327.6}{1132185.6} = s_i = 0.00028935$$

We show that the minimum viable stake for a validator must be $0.029$% of the total stake in the Solana blockchain ecosystem. In other words, there can be a maximum of $3,460$ validators in the ecosystem assuming no subsidies and $0$% commission on inflation. There are currently $1500$ active validators, according to [Solana Beach](solanabeach.io/validators).

As of writing, the total staked pool of SOL sits at approximately $300,000,000$, which gives us a minimum viable amount of staked SOL at $86,700$ per validator. However, we are still missing a key feature.

As previously mentioned, validators become successful when their probability of receiving a block reward goes up. This is intuitive since the server costs $c_s$ and voting costs $c_v$ are fixed. This means a validator is more profitable as the number of SOL staked increases. Validators are able to attract more stake by offering an enticing yield to those who choose to allocate their stake with them, no different to the incentives of a traditional fund manager. Hence, each staked SOL has an opportunity cost $Opp$ associated with it, which the validator must pay out.

![A validator's annual yield of SOL.](/output.png "Annual Yield of SOL")

With a 0.029% stake, profits are zero, and the validator is not able to provide a yield. At above $0.029$%, returns to the validator increase (see the plot above). However, at the current block reward $r_{block}$ and current transaction throughput $EpY$, these returns significantly underperform a very conservative expected return on stake (i.e, 5%) by at least an order of magnitude for all $s_i$. This means that if we include the Opportunity Cost $Opp$ as an expense in our equation $e_i$, the incentives for providing stake to a validator or for a validator providing stake to itself become dependent on inflationary rewards.

**Inflationary Rewards**

Solana has incorporated a dynamic inflation schedule as part of their economic design for validators and stakers. To incentivise early participation in securing the network, Solana pays out an annual inflation to stakers. Solana has set inflation to be decreasing over time, and at steady-state is supposed to be set between $1.5-2$%, starting at $15$% and reducing by $currentinflationrate - currentinflationrate * 0.15$ annually. Currently, inflation is set at approximately $5.5$%, and stakers who choose to stake with validators that are reliably voting with low latency are rewarded by getting a higher percentage of the inflationary reward. For example:

- Validator A: Voting skip rate of 0.2, High Latency (slow)
- Validator B: Voting skip rate of 0.01, Low Latency (fast)

A staker may earn the entire $5.5$% inflationary reward if they stake with the superior Validator B, and only $4.9$% with Validator A.

Validators may charge a commission $r_{comm}$, a percentage of the staker's inflationary reward, to help cover cost with running a validator. As the validator becomes self-sufficient (i.e., with a stake above $0.029$%), then they may move to charging $0$% commission in order to attract more stake, and therefore, earn more block rewards.

Currently, many of the largest validators on Solana charge commission (usually below $10$%): a likely reason is that the revenue on inflation commission far outweighs the potential block reward and current transaction throughput. A key question is what happens when inflation reaches Solana's target $1.5$% annual steady state?


TODO: 
- Annual yield reaches a steady state at around 0.003 stake. However, validator rewards increase significantly, assuming they do not need to share the block rewards (need to plot this).
- What happens at 1.5% inflation, charge 100%, 10% or 0%? (Assume stakers will not be satisfied with only 1.5% APY)


**What if transaction throughput increases?**

So far we have assumed that the Epochs per Year $EpY$ are constant at $168$ per year. $EpY$ increases if transactions on the Solana blockchain increase. Since there are 432,000 fixed blocks per Epoch, we can safely assume that there are currently around $432000 * 168 = 72576000$ or $72$ million transactions per year. To put this into perspective, there are approximately 1-2 billion credit card transactions processed per day around the world. It would be sensible to assume that Solana's transaction throughput could easily be $10x$ more per year. So what happens to the incentives of validators if transaction throughout increases?

As $EpY$ increases, the yields of validators also increase dramatically. When we increase $EpY$ $10x$ to $1680$, the returns also increase $10$x for all validators $n$, which is in the ballpark of a more realistic conservative investment expectation. However, the yield function is monotonically increasing. This means that validators with higher stake are guaranteed a higher baseline return. If a validator holds $100$% of Solana stake, they are guaranteed the highest yield. Increasing the block reward has no effect on the monotonic properties of this yield function. 

![A validator's annual yield of SOL given 10x current transaction throughput](/output_10x.png "Annual Yield of SOL at 10x throughput")

**What does this mean?**

Stakers are able to choose which validators they choose to stake with. Stakers are incentivised to choose stakers that a) provide them with the highest yield, and b) have a good track record of performance, security and trustworthiness. In many ways, this is no different to an individual choosing between fund managers: they fund manager that provides the highest guaranteed returns over time is bound to attract the most capital. Unlike fund managers, however, Solana's validators have limited capacity to optimise future returns: both validator costs and block rewards are fixed, and there is no way to optimise yield other than by increasing the proportion of the stake. Thus, stake will tend towards validators that hold the largest stake over time. There will likely be only 1-5 serious validators.

The Solana blockchain is just four years old, and at the time of writing there is a "supermajority" of 19 validators[^4]. Solana Foundation have programmes in place to subsidise voting costs for new validators to facilitate decentralisation of the network[^5]. Additionally, validators are incentivised to use MEV to extract more revenue, and successful validators may earn a significant income in executing MEV successfully, which may partially explain Solana's $1500$ active validators. Yet, it is unclear whether the majority of these validators are experimental or speculative[^6], incentivised by Solana Foundation's voting subsidies, implementing MEV strategies successfully, or if these validators are really independent of one another[^7]. Whatever the reason is, this theoretical analysis has found that the foundational economic model for Solana validators incentivises centralisation and we expect centralisation over time. 

One big caveat is that we have not incorporated the incentives of MEV revenue generated by validators. The reason for MEV not being included in this analysis is because MEV is highly variable and dependent on the algorithmic sophistication of a validator, in addition to the stake they are able to attract. We also suspect that MEV may have other [peverse outcomes](https://eprint.iacr.org/2024/331) on the viability of the Solana blockchain ecosystem, explored later in [Part Two: JITO Labs & MEV].

**Conclusion**

Solana is touted as a low-cost, fast and decentralised crypto ecosystem. It currently boasts one of the largest crypto ecosystems, supported by a large marketing budget and considerable support for start-up applications building on the Solana chain. Solana's point of difference is based on a flawed *Proof of History* model which enables low transaction fees and fast processing times, arguably the two core disadvantages of the Bitcoin and Ethereum blockchains. But, we must ask: *at what cost?*. 

At present transaction throughput, the voting costs associated with validating the Solana blockchain (due to Proof of History system architecture) makes it economically infeasible for validators to generate non-negative income when considering a conservative return on the SOL staked with a validator. Increasing transaction throughput (or block reward) by only 10-100x improves yield to practical levels. Visa has 0.72 billion transactions per day, so large numbers are not far-fetched. However, even at high transaction numbers, large validators are exponentially more viable than small ones. If this were then the case, Solana Labs would need to reconsider their similarity to transaction giants Visa and Mastercard.

[^1]: https://forum.solana.com/t/proposal-for-enabling-the-reward-full-priority-fee-to-validator-on-solana-mainnet-beta/1456/88
[^2]: https://cogentcrypto.io/ValidatorProfitCalculator
[^3]: Note that validators are required to vote no matter their stake. Validators that miss voting are punished by way of voting credits. For more, see: 
[^4]: https://solanabeach.io/validators
[^5]: https://solana.org/delegation-program
[^6]: {If we do not consider MEV, SOL profits generated from holding a stake over 0.03% may be desirable *if* SOL appreciates in value (e.g., in terms of USD) faster than a conservative annual return on investment such as 5%. This is an entirely feasible scenario, but likely not sustainable and precarious especially if Solana is supposed to be used for purchasing goods and services.}
[^7]: Solana Labs are likely also incentivised to run their own validators with no immediate reward mechanism for running them, since the success of the overall ecosystem would provide economic returns in other ways.
[^PWC]: https://www.ey.com/en_us/insights/financial-services/an-introduction-to-maximal-extractable-value-on-ethereum
