# EOS.IO Chain Resources

This document describes the three resource types that are essential to the nominal function of an EOS.IO blockchain:
Bandwidth, Data, and Voting Rights.

In shared distributed systems, like a blockchain, the economy that dictates how shared resources are allocated and used
is essential to the systems health.  A poorly balanced system of resource allocation can lead to user frustration and in
some extreme cases denial-of-service.  It is therefore essential that the mechanisms in place to balance these resources
are well documented and understood.

In EOS.IO, each of the three primary resource types has its own set of rules that align the proper usage of those
resources with the best interests of the chain and stake holders.  In doing so, EOS.IO presents a fair and
self-sustaining model that protects itself from abuse and ultimately creates value for all users who share the
blockchain.

## Overview of Resources

### Bandwidth

Bandwidth is an umbrella term for any incidental resource consumed while processing transactions into the blockchain.
This includes computational resources such as CPU, as well as network resources such as capacity on the peer-to-peer
network for synchronizing a given transaction to all producers and non-producers. Finally, this protects the resource of
space within any given block.  All of these resources, once consumed, have no recurring cost for an in-sync node.

### Data

Data is an umbrella term for the persistent resources consumed while processing transactions into the blockchain.  When
smart contracts write to the blockchain's database they consume Data.  Additionally, some actions, like creating a new
account or a deep permission structure on an existing account will also consume Data.  These resources have a recurring
cost on an in-sync node as those nodes must maintain a commitment to maintain their state until they are released.

### Voting Rights

Voting Rights are a virtual resource that reflects an individuals stake in the future value of an EOS.IO blockchain.
Users who are willing to "bet" on the long-term value of the blockchain are awarded the ability to vote on issues that
affect that long-term value directly.  At the present time, this is used to elect the producers entrusted with
maintaining the operational semantics of the chain to the betterment of all stake-holders.

## The Bandwidth Resources

Bandwidth usage is incidental.  It is essential at the point in time where a transaction is processed and yet it has
only minor costs on long-term storage and syncing new nodes on the peer-to-peer network  once that transaction is
processed.  It is important to balance this resource as that system of balance will dictate how the blockchain behaves
in times of heavy congestion.

Existing, fee-based blockchains rely on the willingness of users to bribe custodians of the blockchain with higher fees
in competition with all other would-be users in a given time slice. This aligns the incentives of those custodians such
that they will always want the highest level of congestion the fee-market will bear.

Custodians, aka Producers for EOS.IO, should have incentives to increase capacity of the blockchain to meet demand, not
artificially limit it to extract higher fees on the users.  To this effect, EOS.IO has no fees.  However, fees do serve
as a mechanism of spam prevention, making it economically infeasible to flood the network.  EOS.IO has an equivalent
economic hedge against network floods discussed below.

The primary Bandwidth resources are space in the blocks and computational time when processing transactions.

### Basic Model

#### Allocation

Ideally, a user's minimum Bandwidth is allocated based on the ratio of EOS.IO tokens a user has staked into the
Bandwidth Resource Pool to the total number of EOS.IO tokens _staked into the Bandwidth Resource Pool_.  For example, if
Alice has staked 10 EOS and there are a total of 100 EOS tokens staked in the Bandwitdth Resource Pool, she may consume
up to 10% of the available Bandwidth resources.

A user's maximum Bandwidth is a multiple of this minimum Bandwidth as voted on by the Producers.  In times of abundance
a user with a modest stake should be able to consume Bandwidth at a rate well in excess of their minimum.

In a perfect world the producers would have all the information the need to enforce this.  Practically, at the time a
Producer must decide to schedule a transaction it cannot know the transactions that will arrive in the future but within
the time span of the current block.

As such, Producers calculate a virtual Bandwidth which over-subscribes the actual capacity of the blockchain by a factor
with a minimum bound of 1 and the the per-user maximum multiple.  For every block produced while the average usage
is greater than the ideal target usage, this factor is reduced by 1%.  For every block produced while the average usage
is less the ideal target usage, this factor is increased by 0.1%.

The ideal target usage is set such that there is a burstable capacity of 10 times the ideal target before the hard
limits voted on by Producers are breached.  This ideal target is the sustainable Brandwidth rate for the existing
blockchain.

This creates an elastic supply of virtual Bandwidth that approaches a strict stake-based allocation under congested
conditions and relaxes to an unbounded first-come-first-serve allocation when the blockchain is idle.

Whether a blockchain is over or under the ideal Bandwith usage is determined using an ideal target usage which is voted
on by the producers.

#### Delegation

Any user may stake tokens into the Bandwidth resource pool of any user, including their own.  This Bandwidth may be
instantly used by the owner of the Bandwidth resource pool that received the stake and may only be unstaked by the
user who supplied the tokens.  Therefore, the staking user retains ownership of the tokens.

#### Locking

In order to prevent abuses where a user stakes EOS, uses some incidental resources, un-stakes EOS and gives it to
another user to "double dip" on incidental resources.  The allocation of Bandwidth is effectively held in escrow by the
chain for a period of time after it is un-staked from the Bandwidth resource pool.  This locking period is voted on by
Producers and defaults to 3 days.

A user may only have 1 outstanding escrow per-staked Bandwidth resource pool.  If a user attempts to unstake token
while a pending escrow is underway, that escrow is cancelled and the combined value of the escrow and the newly unstaked
tokens is added to a new escrow which has a fresh locking period.  For example, if 2 days ago a user unstaked 10 tokens
and today they issue an action to unstake 5 more tokens, they will effectively receive 15 tokens in 3 days.


### Analysis/Use Cases

#### Attempting to Flood the Network

With the model above, a spam attack which is successful depends on owning _all_ of the EOS tokens.  With any lesser
stake, the spammer can attempt to flood the network and, if they have a significant stake they may be able to trigger a
congested state where the Producers will swiftly reduce the virtual Bandwidth to a factor approaching 1.  As that
happens, all non-spam stakes will guaranteed capacity approaching the proportion of their stake.  The only way to deny
other users for more than a brief period of elastic adjustment is to own 100% of the staked EOS.  Considering any EOS
holder can stake additional EOS to guarantee a portion of the Bandwidth, the attacker is forced to buy and stake _all_
EOS which is economically infeasible.  In addition, any party with a significant enough stake to affect the network in
the short term would be sustaining significant economic damage to that stake in the process.

#### A Market for Burst Capacity

When a user with a low average consumption finds themselves in need of a large amount of Bandwidth for a short period of
time, their best option may be to find a user who is willing to stake tokens into their pool for a modest fee that
covers the "cost" of carrying temporarily locked tokns.  On-chain contracts can guarantee this transaction is trustless
and the market decentralized.

Another way to look at this is not a large amount of Bandwidth but a greater guarantee of instant inclusion in the chain
such as an urgent message.

This adds an ability to EOS.IO equivalent to a fee-market's ability to service extremely important message given a cost.
The primary difference between this ability and other fee-market blockchains, is that the benefactor of the fee is a
stakeholder who's incentive is increasing the blockchains value and not a miner/custodian whos incentive is to extract
value from the blockchain.  Additionally, it does not do so at the expense of other users who still retain their
guaranteed minimum Bandwidth.

#### Distributed Apps with Free Tiers

A DApp can use funds from its own account to stake Bandwidth on its users accounts as part of a free tier of service.
Unlike fee-market blockchains, where a DApp would have to pay fees directly and thereby co-sign transactions for an
equivalent tier of service service, EOS.IO's mechanics allows free-tier users of a DApp to transact efficiently and in
parallel.  At no time, is the DApp at risk of user's absconding with the funds that back this free tier of usage.

## The Data Resource

Data usage is persistent.  When it is consumed, or locked, it carries a recurring expense until it is released.  It is
important to balance this resource as that system of balance will play a large role in the expense of maintaining the
blockchain as a shared resource going forward.

Existing fee-based blockchains rarely include a recurring "rent" payment for resources that carry a recurring expense.
Largely the problem is how to handle a case when an account becomes overdrawn.  In leiu of a "rent" payment, fee-based
blockchains must rely on over-charging when Data is consumed as a protection against abusive usage patterns.

EOS.IO has no fees, instead the incentive against abusive Data usage is based on opportunity cost of having staked and
therefore illiquid EOS tokens.  Combined with an algorithmic market-maker which removes the ability to profit from
speculation on Data as a resource, this creates a model where consuming Data resources on the shared blockchain
indirectly compensates all other stakeholders by temporarily, or permanently, reducing the supply of liquid EOS tokens.

### Basic Model

#### Allocation

Data capacity for an account is bought and sold on a constrained market where pricing is set algorithmically and all
transactions are with the system and a single user.  This is designed with the following principles in mind:

 * Reserving all of the available Data should require staking all of the existing EOS tokens
 * Average and small users will need some Data, so the price should always be reasonable for small capacities
 * Users should be incentivized to reserve Data they need, no more.
 * This is not a market where speculation or profiteering is appropriate.

When a user buys Data capacity, some amount of EOS (the price) is staked on their account.  The ratio of Data used to
Data capacity for a user is held exactly equal to the ratio of EOS locked to EOS staked.  Which is to say, once reserved
capacity is actually used, an equivalent proportion of the staked EOS becomes locked.  This EOS can only be unlocked by
removing usage.  Any staked, but unlocked EOS can be unstaked by releasing Data capacity back to the market. Users cannot
sell Data capacity to each other.

It should be apparent from this model that the quantity of EOS the user controls never changes, it is merely staked or
locked.  As a result, there is never a situation where a user may get more or less EOS from the market than they put in
to it.  Essentially, this removes any capacity for profit from speculation on the Data market.  A user can not "buy low
and sell high" as there is no way to "sell" at a market rate, the only option is to release.

Lastly, purchases on the market are rate-limited to one per account per day to reduce the incentive for rapid micro-buys
which may cause excessive price fluctuations or be used to attempt to exploit the algorithmic nature of the market
maker.

#### Pricing

UNDER REVIEW / COMING SOON

#### Data Usage

Data usage is controlled by contracts on an EOS.IO blockchain.  When a contract needs to store persistent information
it will write to the blockchain database using the appropriate in-contract APIs.  These APIs provide the ability to
specify which account is "billed" for the Data usage.  When a record in the blockchain database is removed, the account
which is currently being billed for the Data usage is refunded an equivalent amount of capacity.  In certain cases, a
contract may update a data record and change the billable account.  In those instances, the billable account prior to
the update is refunded the capacity required by the update and the new billable account is billed for the usage of the
updated record's new data.

### Analysis/Use Cases

#### Distributed Apps with Free Tiers

In some cases, a DApp may want to provide a free-tier to their users but still require some amount of Data in order to
achieve the desired experience.  In these cases, the DApp contract(s) can be authored in a way such that the "scope" on
which the data is stored is that of their users' accounts however, the account which is billed for the storage belongs
to the DApp itself.  As the DApp's contract code is in complete control, the DApp itself can reclaim this usage from
users as is necessary so, there is no risk to a DApp to provide such a service other than the natural cost of the Data
usage itself.

## Voting Resources

Voting resources are used to elect Producers.  As Producers are custodians of an EOS.IO blockchain and play a large
role in the intrinsic value of that chain, Voting resources are considered highly valuable.  As part of the dPos
concensus mechanism at the core of an EOS.IO blockchain, Producers must be aligned with the stakeholders on the chain
who are in turn aligned with the incentive to increase the value of the native EOS token.  This economic loop drives
the blockchain.

### Basic Model

#### Allocation

As Voting resources have a direct impact on the long term value of the chain through election of Producers, it stands to
reason that they are allocated based on the willingness to take a long position in the native EOS token.  As such, users
may increase their respective Voting power by locking, thereby making illiquid, some amount of EOS tokens for a period
of time.  During this period of time, the locked tokens cannot be used to purchase or stake any other resource.  They
may not be traded to another account.  They are, for all intents and purposes, deeply frozen assets.  This in turn,
creates incentive for users to vote according to the best value for the blockchain and as a result the future value of
their locked tokens.

#### Voting Weight

The weight of a vote is the product of a users allocated Voting resources and a perpetually increasing exponent.  This
weight is set at the time a vote is cast and does not change while that vote remains untouched.  The perpetually
increasing exponent is scaled such that the effective weight of a stagnant vote is reduced by 50% every 6 months.  Any
action will recalculate the weight of a vote, including re-confirming that the vote is assigned to a given Producer.
This is designed to ensure that idle or apathetic voters cannot perpetually affect the system.  The burden of renewing a
voting position which is unchanged is minimal and in the best interest of the stakeholders as it represents more recent
and ideally a more informed position.

### Analysis

#### Voter Apathy

Given the decaying relative weight of a vote over time, an apathetic voter takes on a greater economic risk than an
active voter.  Eventually, their vote will be effectively worthless however, they are still taking a long position and
as a result the risk of that long position with less control over the value of their asset.  This presents an economic
incentive to continually review and refresh a voting position or otherwise presents a pressure to exit the voting pool.

#### Intrinsic Value of Votes

In previous iterations of the EOS.IO staking model, Voting resources were conflated with other stakable resources.  This
presented a value model where Votes had some intrinsic value outside of the long term value of the blockchain's native
token.  As such, actors who had a short-term need for resources but no long-term concerns could have incentive to vote
counter to the long term value of the chain, using the effective reduction in price of short-term resources they valued
as a hedge against the long-term losses.  As this is obviously counter to the stated goals, EOS.IO shifted to a model
where Voting Resources only derive value from the long term prospects for the value of the native token.