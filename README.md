<!-- solve-problem:d8ee5935-e1d6-4cea-9b47-f432e13c93f4 -->
# Collusive and malicious parallel-fee attacks

A [Tangle Solve problem](https://solve.tangle.tools/problems/d8ee5935-e1d6-4cea-9b47-f432e13c93f4).

## What is the problem?

**Setting.** A blockchain charges transaction fees under a *multiresource fee model* (fees priced over multiple execution resources — e.g., CPU, memory, bandwidth — rather than a single scalar gas). The block scheduler is a strategic agent who chooses which pending transactions to include and in what order; users are strategic agents who choose fee bids and transaction content. The *parallel-fee* question is whether the fee mechanism makes honest, individually rational behavior incentive-compatible for each party in isolation.

**The open extension.** Two relaxations of the rationality assumption:

1. **User–scheduler collusion (off-chain side payments).** A coalition of users and the scheduler sign off-chain agreements: users get prioritization/inclusion, the scheduler receives bribes. Formally: is the mechanism *collusion-proof* (in the sense of no coalition can profitably deviate with a budget-balanced side-payment vector) in addition to individually incentive-compatible?

2. **Malicious (Byzantine) attackers.** An attacker with budget $B$ willing to *lose* money to degrade liveness/availability — e.g., spamming the scarce resource dimension to starve honest transactions. Formally: bound the maximum capacity denial (excluded fee value of honest transactions) an attacker achieves as a function of $B$, fee structure, and scheduler incentives; or design fees such that denial is linear in $B$ (or better).

The census is informal; the reasonable formalization is a game $\Gamma = (N, \{u_i\}, M)$ where $N$ includes users, scheduler, and attacker types, $M$ is the multiresource fee/scheduling mechanism, and the goal is to characterize equilibrium outcomes and worst-case availability. Exact model details (a specific fee formula, scheduler strategy space) are not specified and must be pinned down from the cited paper.

## Why does it matter?

Blockchain availability (liveness) is a core guarantee, and pure incentive compatibility for individual rational agents does not survive contact with bribery or griefing — both are observed in practice (MEV bribes, fee-market spam). A rigorous extension would tell protocol designers how to set multiresource fees so that (a) collusion yields no surplus, and (b) denial-of-service costs scale fairly with attacker budget. It also connects to the broader theory of mixed Byzantine–rational fault tolerance in resource allocation.

## What's already known?

The census cites a single anchor: Wadhwa, Yaish, Zhang and Nayak (2026), *Perils of Parallelism* (arXiv:2604.04193 — identifier appears future-dated; verify), which analyzes parallel-fee mechanisms under **individually rational** users and scheduler — presumably showing incentive compatibility (or its failure) for non-colluding, non-malicious agents. That analysis **explicitly excludes** collusion and malicious DoS, which is exactly the gap. Related literature threads to draw on (from general knowledge, not the census): MEV/bribery models (e.g., side-payment robustness in transaction fee mechanisms like Roughgarden's EIP-1559 analysis, which partially addresses off-chain agreements), and BAR-tolerance / Byzantine resource pricing. No collusion or griefing bounds appear to be known for multiresource fees specifically. Start from the Perils paper's model and check what breaks.

## What would count as solving it?

Any of the following, in increasing strength:

1. **Impossibility:** A proof that no multiresource parallel-fee mechanism can be simultaneously collusion-proof against user–scheduler coalitions and individually incentive-compatible (with formal definitions).
2. **Positive construction:** A fee/scheduling rule with a proof that (a) no coalition of size ≤ k with budget-balanced side payments gains, and (b) an attacker spending $B$ can exclude at most $f(B)$ honest fee value, with explicit $f$.
3. **Tight bounds:** Matching upper and lower bounds on cost-of-attack $f(B)$ for a natural mechanism class (e.g., showing denial is $\Theta(B)$ in one resource dimension but unbounded in another).
4. **Simulation/experiment (weaker):** A concrete mechanism stress-tested against collusive and griefing strategies, with quantified availability loss — acceptable only if theory is the long-term goal and stated as such.

Definitions must be fixed first: collusion-proofness notion (weak vs. strong), attacker's information (does it observe the mempool?), and the scheduler's objective.

## How to start

1. **Read *Perils of Parallelism* first**, extracting its exact model: resource vector, fee rule, scheduler strategy space, and the individual-IC result. Every subsequent step is relative to this model.
2. **Import collusion machinery from the TFM literature:** Roughgarden's transaction-fee-mechanism papers formalize "no off-chain agreements" (verify which paper introduces the OCA-proofness notion — likely *Transaction Fee Mechanism Design* 2020/2021). Test whether their impossability/possibility results port to the multiresource setting; the parallel structure may either enable or destroy collusion-resistance.
3. **For the malicious side:** model the attacker as a budget-constrained adversary in the fee game and compute the price of denial per unit of scarce resource; compare across resource dimensions — the interesting pathology should be that cheap-to-price dimensions are expensive-to-schedule ones.
4. **Main obstacle:** the scheduler is a single powerful agent, so scheduler-inclusive coalitions are typically *not* preventable by fees alone; expect an impossibility result plus a mitigation (e.g., commit randomness, rotating schedulers, or burning fees).

## Key references

- Wadhwa, S., Yaish, G., Zhang, C., & Nayak, K. (2026). *Perils of Parallelism*. arXiv:2604.04193. Establishes the individual-rationality parallel-fee analysis that this problem extends; the baseline model. (Identifier could not be resolved; verify.)
- Roughgarden, T. (2020/2021). *Transaction Fee Mechanism Design for the Ethereum Blockchain*. arXiv:2012.00854. Defines TFM desiderata including approximate MIC, UIC, and OCA (off-chain agreement) resistance for EIP-1559-class mechanisms — the natural collusion framework to port. (verify)
- Amdur-Clark & Roughgarden or Ferreira et al. on fee markets / MEV bribes — survey candidates for the collusion attack model. (verify; no specific census citation)

## Warnings

- The anchor citation is future-dated (2026) and could not be verified; do not assume its contents — extract the actual model before building on it.
- The census record is vague: "parallel-fee analysis" is not a standard term. The brief above formalizes the most plausible reading; confirm definitions from the source paper.
- Common dead end: trying to make the *scheduler* fully collusion-proof with on-chain fees alone — bribes are off-chain, so some impossibility is expected; frame results as bound-or-mitigate, not eliminate.
- Watch the griefing metric: "wasted capacity" can be measured in block space, honest excluded fees, or delay; fix one and state it.

---
*`BCGT-000292` · unverified*

## Acceptance guidance

Any of the following, in increasing strength:

1. **Impossibility:** A proof that no multiresource parallel-fee mechanism can be simultaneously collusion-proof against user–scheduler coalitions and individually incentive-compatible (with formal definitions).
2. **Positive construction:** A fee/scheduling rule with a proof that (a) no coalition of size ≤ k with budget-balanced side payments gains, and (b) an attacker spending $B$ can exclude at most $f(B)$ honest fee value, with explicit $f$.
3. **Tight bounds:** Matching upper and lower bounds on cost-of-attack $f(B)$ for a natural mechanism class (e.g., showing denial is $\Theta(B)$ in one resource dimension but unbounded in another).
4. **Simulation/experiment (weaker):** A concrete mechanism stress-tested against collusive and griefing strategies, with quantified availability loss — acceptable only if theory is the long-term goal and stated as such.

Definitions must be fixed first: collusion-proofness notion (weak vs. strong), attacker's information (does it observe the mempool?), and the scheduler's objective.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/d8ee5935-e1d6-4cea-9b47-f432e13c93f4?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000292.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000292)
