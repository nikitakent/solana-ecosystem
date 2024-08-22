# The Flawed Economics of Solana

## Part One: The Validators

Solana is a blockchain ecosystem with a market capitalisation of US$67 billion at the time of writing. Solana consistently fluctuates between the fifth and sixth largest crypto ecosystems of all time. Solana was launched in 2020 as an alternative to Ethereum, offering smart contract and NFT solutions without the high fees or slow transaction waiting times. The native token that runs on the Solana blockchain is called SOL.

Solana uses a consensus mechanism called *Proof of Stake*, which it uses in conjunction with a system design technique called *Proof of History* (later re-developed to become *Proof of Replication*, but to avoid confusion, we will stick with the name *Proof of History* throughout the paper). The technical details of these mechanisms are unimportant for now, as this analysis focuses on the Economic Incentives of Solana Validators, which secure the system. The most important thing to know for now is that the *Proof of History* model requires validators to vote, costing about $1.95$ SOL per day.

Solana promises a high level of decentralisation. In this analysis I prove that it is not possible - without assuming significant MEV profits and SOL appreciation - to have a high level of validator decentralisation based on their current validator incentive model. My results are backed anecdotally. Despite providing significant voting subsidies and technical assistance to new validators for years, in March 2024 Solana Labs voted to remove the burning of the 50% priority fee so that validators may have more economic incentive to secure the system. However, such a move would compromise the integrity of the entire ecosystem, a topic to be covered for another day.

**Game Setting**

Validators have two key fixed annual costs $e$: voting fees $c_v$ and server fees $c_s$. One may assume that server fees for running a validator on the Cloud run between US$4500 to $6000 per year, however we omit server fees in our analysis for brevity.

- $c_v$ - currently costs $1.95$ SOL per epoch, or around $1.1$ SOL per day.
- $EpY$ - Epochs per Year. The more transactions that occur on Solana, the more epochs there are. Currently there are around 168 epochs per year. We assume that this number is constant in the analysis.
-  $s_{i}$: total stake in SOL for validator $i$ 
- $Opp$ - Cost of stake, or else known as "opportunity cost". Validators often pay a yield to "stakers" that choose to stake with them. Assume for now $Opp = s_i * 0.05$ at a conservative yield of $5$%.

(make footnote?) *Note that validators are required to vote no matter their stake. Validators that miss voting are punished by way of voting credits*

$$e_i = c_v*EpY+ Opp $$
Validators earn revenue by the way of annual block rewards $r_{annual}$. The probability of getting a block reward $r_{block}$ for a validator $i$ is equal to the probability of being assigned the block leader, which is equal to the proportion of the validator's *stake* over the entire pool of staked SOL.

$$
r_{annual} = \frac{s_i}{\sum_{i=1}^{i=n}s_i} * 432,000 * EpY * r_{block}
$$

- Solana has a fixed number of blocks (aka slots) per epoch: $432,000$.
- $r_{block}$ is the current block reward, which is determined by the base fee and prioritisation fees. For the analysis, we assume $r_{block}$ to be constant at $0.0156$ SOL, fluctuating marginally over time.

Validators can also earn revenue via a block ordering technique called **Maximum Extractable Value**. This technique is more controversial than not, since a lot of MEV is exploitative. Not all validators run MEV, and those that successfully run MEV have sophisticated algorithmic bots that beat other validator's bots. I have excluded the inclusion of MEV in this analysis since it is a fraction of what total block rewards generate and is the crypto equivalent to getting a "bonus" (as opposed to a reliable, fixed income).

**Analysis**

To find the minimum viable stake $\hat{s_i} = min{\frac{s_i}{\sum_{i=1}^{i=n}s_i}}$ a validator needs to have in order to have no losses, we derive:
(I like to start with the original equation; words could help to summarise each step)
$$e - r_i = 0$$
$$\hat{s_i}* 432000 * 0.0156 * 168 = e$$
Omitting $Opp$ for now, assume fixed expense for all $i$ at $e = vf * EpY = 327.6$. 

$$1132185.6 *\hat{s_i}=327.6$$
$$ \frac{327.6}{1132185.6} = s_i = 0.00028935$$

We show that the minimum viable stake for a validator must be $0.029$% of the total stake in the Solana Blockchain Ecosystem. In other words, there can be a maximum of $3,460$ validators in the ecosystem assuming no subsidies.

As of writing, the total staked pool of SOL sits at approximately $300,000,000$, which gives us a minimum viable amount of staked SOL at $86,700$ per validator. However, we are still missing a key feature.

As previously mentioned, validators become successful when their probability of receiving a block reward goes up. This is intuitive since the server costs $c_s$ and voting costs $c_v$ are fixed. This means a validator is more profitable as the number of SOL staked increases. Validators are able to attract more stake by offering a yield to those who choose to allocate their stake with them, no different to the incentives of a traditional fund manager. 

![A validator's annual yield of SOL.](/output.png "Annual Yield of SOL")

With a 0.029% stake, profits are zero. At above $0.029$%, returns to the validator increase (see the plot above). However, these returns significantly underperform a very conservative expected return on investment (i.e, 5%) by at least an order of magnitude for all $s_i$. This means that if we include the Opportunity Cost $Opp$ as an expense in our equation $e_i$, the incentives for providing stake to a validator or for a validator providing stake to itself become unclear.

**What if transaction throughput increases?**

So far we have assumed that the Epochs per Year $EpY$ are constant at $168$ per year. $EpY$ increases if transactions on the Solana blockchain increase. Since there are 432,000 fixed blocks per Epoch, we can safely assume that there are currently around $432000 * 168 = 72576000$ or 72 million transactions per year. To put this into perspective, there are approximately 1-2 billion credit card transactions processed per day around the world. It would be sensible to assume that Solana's transaction throughput could easily be $10x$ more per year. So what happens to the Economics of Validators if transaction throughout increases?

As $EpY$ increases, the yields of validators also increase dramatically. 


If the validator offers a 5% yield on the SOL that investors stake with them, at no point is there a non-negative return for the validator.
At a 5% conservative return for a validator's staked SOL, at no point is there a non-negative return for the validator. The same applies if we reduce this return to $1$%.

**Conclusion**

This analysis has examined the basic validator economic model of the Solana validators. The voting costs associated with validating the Solana blockchain (due to Proof of History/Proof of Replication system architecture) makes it economically infeasible for Validators to generate non-negative income when considering a conservative return on the Solana staked with a validator. 

There are two main caveats of this analysis. 

Firstly, this analysis has dismissed the economic incentives of MEV, which provide significant incentives for many validators on the Solana blockchain (Jito Labs). The reason being that MEV is not guaranteed, combined with the personal opinion that the majority of MEV is exploitative and signals a *Lord of The Flies*-esque Economic System. Despite these reasons, I plan to explore the economic incentives of Solana validators when taking into consideration MEV at a later date for completeness.

Second, we have assumed that the current block reward is constant at $0.0156$ SOL per "slot", which could massively increase if Solana were to increase transaction fees (via the base fee or the prioritisation fees). This is entirely possible and could dramatically improve the incentives to become a validator. Already, Solana Labs have decided to provide more return to validators by forgoing the burning of the 50% of the base fees and prioritisation fees. This move has other far-reaching consequences including collusion. A suspicion for Solana not burning the entire base fee is that MEV would be harder for validators to pursue: evidently, the entire Solana ecosystem is dependent on the profits derived from MEV.

Solana is touted as a low-cost, fast and decentralised crypto ecosystem. It currently boasts one of the largest crypto ecosystems, supported by a large marketing budget and considerable funding for start-up applications building on Solana. Solana's point of difference is based on a flawed *Proof of History* model which enables low transaction fees and fast processing times, arguably the two core disadvantages of Bitcoin and Ethereum blockchains. But, we must ask: *at what cost?*

If we do not consider MEV, SOL profits generated from holding a stake over 0.03% may be desirable *if and only if* SOL appreciates in value (e.g., in terms of USD) faster than a conservative annual return on investment such as 5%. This is an entirely feasible scenario, but likely not sustainable and precarious especially if Solana is supposed to be used for purchasing goods and services. Solana Labs are likely also incentivised to run their own validators with no immediate reward mechanism for running them, since the success of the overall ecosystem would provide economic returns in other ways. If this were then the case, Solana Labs would need to reconsider their similarity to transaction giants Visa and Mastercard.

- Add references
- Is MEV a zero sum game when it comes to validators?
- Check this: Already, Solana Labs have decided to provide more return to validators by forgoing the burning of the 50% of the base fees and prioritisation fees.
[i] - have not considered inflationary/deflationary pressures (assumed to be insignificant)


--- Discussion notes
Larger transaction numbers may also make validators economically viable. This will be the main argument of proponents, and it only needs 10-1000x transaction numbers to start making sense.
Visa has 0.72 billion transactions per day, so large numbers are not far-fetched.

Even at high transaction numbers, large validators are exponentially more viable than small ones.

There is an argument to be made about centralization being the inevitable outcome, because larger validators can offer better returns. There will likely be only 1-5 serious validators.