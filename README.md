<!-- solve-problem:6c55924c-f5f9-4fd2-9eac-86217e6ec02a -->
# Incentive-compatible defense adoption

A [Tangle Solve problem](https://solve.tangle.tools/problems/6c55924c-f5f9-4fd2-9eac-86217e6ec02a).

## What is the problem?

**Setting.** Proof-of-work blockchains are vulnerable to *selfish mining* (withholding blocks to waste honest miners' work). The canonical defense line is *backward-compatible tie-breaking* (Eyal–Sirer's "uniform tie-breaking" variant; Heilman's published null-destination addresses): honest miners break forks in a way that neutralizes the attacker's advantage, without requiring the attacker to change anything. These defenses only need honest nodes to upgrade.

**Formal problem.** Model miners as rational expected-utility maximizers with hash shares. A defense mechanism is parameterized by its adoption fraction θ ∈ [0,1] of total hash power. Let R_i(θ, s_{-i}) be miner i's relative revenue when running the defense. Two properties are wanted:

1. **Effectiveness (threshold property):** there is θ* such that selfish mining is unprofitable (relative revenue < fair share) for all θ ≥ θ*.
2. **Incentive compatibility of adoption:** the upgrade itself must be individually rational *at every θ below θ\**, i.e., for a deviating/unilateral adopter or abandoner, running the defense is a best response even while a positive fraction of competitors (including possible selfish miners) keep old software. Equivalently: find conditions under which defense adoption is a (weakly) dominant strategy, or the game has a "all adopt" equilibrium reachable without coordination.

The census asks for an **activation mechanism** (protocol change, fork choice rule, or incentive overlay such as altered coin distribution conditional on software/behavior) satisfying both. The subtlety: the second requirement is distinct from the first and is where existing defenses are not proven safe — adopting early may be costly if θ < θ*.

## Why does it matter?

Every tie-breaking defense against selfish mining has a "valley of death": it only protects miners once enough hash power runs it, so a rational miner who believes adoption will stall should free-ride or defect, which can stall adoption exactly when it's needed. A mechanism making adoption incentive-compatible at all θ would let defenses deploy through ordinary self-interested software choices rather than social coordination, and would generalize to other threshold defenses (withholding attacks on Ethereum, cross-chain attacks). Without it, the anti-selfish-mining literature produces mechanisms that are provably effective yet plausibly never deployed — a persistent gap between theory and practice in blockchain security.

## What's already known?

- Eyal and Sirer (2014) established selfish mining's profitability and noted uniform tie-breaking raises the required attacker hash share from ~25% to ~33% (still below 50%).
- Heilman (2014) proposed honest mining with published null-destination addresses; both defenses are backward compatible (effective against unmodified attackers) but need honest-majority *adoption*.
- Zhang and Preneel (2017), *Publish or Perish*, improved tie-breaking to push the profitability threshold to ~25% attacker share (verify exact figure) — but again assume adoption, not incentives to adopt.
- Finkbeiner et al. (2025), IACR ePrint 2025/043, studies defense/adoption incentives in this lineage; its precise contribution should be read first-hand (verify — treat as the closest prior work).
- General technology-adoption games and threshold public goods (e.g., "chicken" and assurance-game structures) predict adoption failure below the threshold; no one has, to our knowledge, closed the gap by exhibiting a mechanism where adoption is weakly dominant for PoW tie-breaking defenses.

Why prior approaches fall short: they analyze *effectiveness given adoption*, treating adoption as exogenous coordination. The adoption subgame — where a lone defender faces mixed population including a possible selfish miner — is rarely modeled, and in the naive tie-breaking rules a lone adopter can earn strictly less than a non-adopter (losing ties it would otherwise win), making non-adoption the dominant strategy.

## What would count as solving it?

Any of the following, in decreasing strength:

1. **Mechanism design (positive):** a concrete protocol modification M and a proof (in a formal mining game with rational miners and a possibly selfish minority) that running M is weakly dominant — or that "all adopt M" is the unique/risk-dominant equilibrium — while M still renders selfish mining unprofitable above its threshold θ*. Bonus: small θ*.
2. **Impossibility (negative):** a theorem that no backward-compatible tie-breaking-style rule can simultaneously be individually rational to adopt below θ* and effective above θ*, under a stated class of mechanisms — this would formally establish the coordination requirement.
3. **Intermediate:** characterize the adoption game's equilibrium structure (e.g., conditions under which adoption is an assurance game with a high-risk/high-payoff equilibrium) and design *activation* machinery (e.g., miner-activated forks with lock-in thresholds, commitment deposits) that implements the good equilibrium.

A referee would want: a formal game specification, proofs or a rigorous simulation study with worst-case attacker behavior, and explicit assumptions (network position, timing, fee/block reward model).

## How to start

1. **Read Finkbeiner et al. (2025) first** — it is the most recent work on exactly this question and will define the current game model; check whether it already proves a partial positive or negative result so you don't duplicate it.
2. **Read Zhang & Preneel (2017)** to internalize the strongest existing tie-breaking rule; recompute the *deviating miner's* revenue (not the attacker's) as a function of adoption fraction θ. This one plot — defender payoff vs. θ against a selfish minority — is the crux: it reveals whether adoption is chicken, assurance, or Prisoner's Dilemma.
3. **Model as a threshold public good / club good.** Ask what instrument can change payoffs below θ*: e.g., conditional coinbase payouts to identifiable defense-running miners, commit-reveal fork-choice weights, or an MMR-style signaling scheme. The obstacle: behavior-based rewards are hard to make sybil-proof and backward compatible at once.
4. **Check impossibility early.** A short argument that unilateral adoption is never strictly profitable under any backward-compatible rule would be a publishable negative result and would redirect the field toward coordinated activation (UASF-style).

## Key references

- I. Eyal, E. G. Sirer (2014), *Majority is not Enough: Bitcoin Mining is Vulnerous*, Financial Crypto 2014, arXiv:1311.0243. — Defines selfish mining, its profitability threshold (~25–33% depending on tie-breaking), and the uniform tie-breaking mitigation; the source problem statement.
- E. Heilman (2014), *One Weird Trick to Stop Selfish Miners: Fresh Bitcoins, a Solution for the Honest Miner*, FC 2014 Bitcoin Workshop. — Proposes null-destination-address tie-breaking to deter selfish mining, still requiring honest-majority adoption.
- Z. Zhang, B. Preneel (2017), *Publish or Perish: A Backward-Compatible Defense Against Selfish Mining in Bitcoin*, ACNS 2017, DOI:10.1007/978-3-319-52153-4_16. — The strongest backward-compatible tie-breaking defense; pushes the attacker profitability threshold down (i.e., requires larger attacker share), but assumes rather than incentivizes adoption.
- Finkbeiner et al. (2025), IACR ePrint 2025/043, https://eprint.iacr.org/2025/043. — Recent work on defense adoption incentives in this lineage; read first-hand to establish the current state of the art (verify contents — cited from census, not independently confirmed).
- (Background, generic) Literature on threshold public goods and technology adoption games — standard game-theoretic framing showing under-provision below thresholds; select a canonical text when formalizing.

## Warnings

- **Do not conflate effectiveness with adoption incentives.** Every paper in the "backward-compatible defense" line solves the first; the census problem is precisely the second. A result of the form "the defense works once everyone runs it" is not an answer.
- Backward compatibility is one-directional: the defense works against old *attackers*, but old *honest* nodes interacting with new honest nodes can create payoff asymmetries — this is likely where non-adoption becomes dominant; check tie-breaking interactions between heterogeneous clients carefully.
- Tie-breaking rules often assume a specific network model (γ parameter for propagation races); adoption incentives may flip depending on γ, so state it.
- The census record is thin on the Finkbeiner et al. (2025) contents — verify before building on or citing its results.
- Simulation studies are only convincing with worst-case (adaptive) attackers and explicit fee models; naive Markov-chain revenue analyses omit fees and difficulty-adjustment transients that can dominate a deviating miner's short-run payoff.

---
*`BCGT-000237` · flagged for review*

## Acceptance guidance

Any of the following, in decreasing strength:

1. **Mechanism design (positive):** a concrete protocol modification M and a proof (in a formal mining game with rational miners and a possibly selfish minority) that running M is weakly dominant — or that "all adopt M" is the unique/risk-dominant equilibrium — while M still renders selfish mining unprofitable above its threshold θ*. Bonus: small θ*.
2. **Impossibility (negative):** a theorem that no backward-compatible tie-breaking-style rule can simultaneously be individually rational to adopt below θ* and effective above θ*, under a stated class of mechanisms — this would formally establish the coordination requirement.
3. **Intermediate:** characterize the adoption game's equilibrium structure (e.g., conditions under which adoption is an assurance game with a high-risk/high-payoff equilibrium) and design *activation* machinery (e.g., miner-activated forks with lock-in thresholds, commitment deposits) that implements the good equilibrium.

A referee would want: a formal game specification, proofs or a rigorous simulation study with worst-case attacker behavior, and explicit assumptions (network position, timing, fee/block reward model).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/6c55924c-f5f9-4fd2-9eac-86217e6ec02a?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000237.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000237)
