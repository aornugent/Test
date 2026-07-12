# System-design evaluation

**A least-doing architecture for identifying θ and selecting the governing operator of a threshold-forced, fast–slow, structured-transport system** — evaluated against `requirements_v2_abstract.md` under the `system-design` skill.

> **This supersedes the first draft in this file's history.** That draft committed to an eight-part reduction stack (slave w, precompute routing, amplitude equation, adjoint-at-attractor, self-consistency fixed point, …). Re-run under the skill's actual procedure, that draft is `examples.md`'s annotated failure #3: *adjoint-at-equilibrium chosen on inherited scarcity, candidates arranged around the favourite.* The corrected verdict is below. The reduction stack is not deleted from the world — it is **demoted to conditional branches**, each gated on a number the requirements withheld.

## Method (why you can trust the reversal)

Triage put this at **Tier 3** (reversal cost = a whole compute architecture; requirements arrived as solution-verbs), which mandates a **deep search**: one context cannot produce independent candidates or grade them honestly. So — three proposers each ran the procedure in an **isolated context** under a **different assigned framing move** (Move 3 boundary/buy, Move 2 record→replay, Move 6 Pólya/fixed-point), seeing only the outcome-form ledger, never each other or the favourite. A **fresh judge with kill authority** then derived the scarce resource independently, ran an arithmetic pass and each proposal's own kill question, and ranked by ledger. The winner matching `examples.md`'s "floor wins" ending is a flag, not a comfort — so scarcity was **re-derived from ledger quantities** (below), not inherited; the match is earned.

---

## Triage
**Tier 3.** Reversal cost is a research/compute architecture with external consumers of the identified model. The source requirements arrived as mechanisms wearing requirement clothes — Turing/Swift–Hohenberg, a routing operator, pullback attractors, a Lyapunov-gradient, "likelihood-free inference." Every one was stripped to an outcome and challenged upward before any design existed.

## Requirements ledger (compressed) + scarce resource

Solution-verbs stripped; quantity or `unknown — ask` on each; the source's P1–P5/§3 become outcomes R1–R7.

- **R1** — reproduce finite-wavelength patterning *from the u–w coupling* (not from the substrate φ), with hysteresis/history-dependence and drift on a tilt. *(stripped: amplitude equation)*
- **R2** — response depends on the forcing **distribution**, not its mean (a threshold opens a Jensen gap), and downscales coarse forcing to resolved local input via the fixed substrate. *(stripped: precomputed routing operator)*
- **R3** — θ **shared domain-wide**, **p ≈ 50**, effective **d_eff < 50** (`unknown`), over-determined; φ / coefficients / forcing-law are fixed, not fitted.
- **R4** — valid at the attractor **and** along transients, including drift **through** the threshold; yields **rates** (relaxation, distance-to-threshold, critical slowing) and answers tracks-vs-tips. *(stripped: pullback / rate-tipping machinery)*
- **R5** — a distinguished θ sits at a critical point of a **self-consistent** growth-rate functional. **Constraint-or-hypothesis is UNRESOLVED and high-leverage** — the source says θ is only *"hypothesised"* to sit there.
- **R6** — output a **calibrated posterior over θ** + a **model verdict** over a small operator set (~2–5), from **≥ 2 joint** functionals, licensing **extrapolation** beyond sampled forcing; no tractable likelihood → likelihood-free. *(stripped: SBI as a named mechanism)*
- **R7** — the covariate that would "explain" θ(x) (the R2 effective-input field) is **endogenous** → any regression on it is circular → reject.

**Scarce resource (one sentence, derived — not inherited from §4's prose):**
> Total forward-model compute = **(cost of one integrate-to-attractor of the stiff, ε≪1, 2-D pattern-forming PDE through its hysteretic threshold) × (number of forward/sensitivity evaluations the p≈50 likelihood-free inversion + model-selection demands)** — and §4 leaves **both factors open**, so the product is worst exactly where R1/R4 make ∂functional/∂θ **ill-posed (divergent at onset, non-unique in the multistable region)**, and its magnitude **cannot be computed until `C_fwd`, `N_eval`, and `d_eff` are supplied.**

All four derivations (three proposers + judge + the sealed orchestrator sentence) agreed on this product independently. Its corollary is the decisive instrument: **the floor cannot be killed by arithmetic, because the killing arithmetic needs numbers the requirements withheld.**

---

## The floor — verdict: **WINS**

The dumbest design meeting the ledger: wrap the PDE system as a **black box** `S(θ, 𝒫, m) → F` (state functionals), hand it to an **off-the-shelf** simulation-based-inference engine → per-operator posterior `p(θ | F, m)` + a model-selection score; calibrate with SBC; extrapolate by **re-simulating** at θ̂ under out-of-sample forcing. Roughly zero new names. R1/R2/R4 are *behaviours of S*, not machinery we build; **R7 is discharged for free** because the endogenous effective-input field is internal to S and is never exposed as a regressor. It cannot be eliminated by arithmetic (`C_fwd`, `N_eval`, `d_eff` withheld). **The floor is the design.**

## Candidates — one line each

- **A — Move 3 (boundary / buy):** inference touches the physics **only** via `(θ,𝒫,m)→F` (no adjoint / amplitude eqn / Lyapunov gradient) — pays **R6** (posterior + model choice bought), **R7** (endogeneity internal → non-circular), **R3** (p≈50 native to sequential SBI) — **wins when** the forward solve is cheap–moderate and no trustworthy analytic reduction exists.
- **B — Move 2 (record→replay):** tabulate **only** the exogenous, θ-independent redistribution (`b = D[𝒫;φ,Θ]`, `R_φ = factorize(L_φ)`), keep the endogenous θ-live u↔w solve — pays **C_fwd / R2** by lifting the nonlocal gated downscaling out of the inner loop, amortised across θ-evals at fixed forcing — **wins when** that redistribution is a genuine global solve *and* many θ-evals share one forcing scenario.
- **C — Move 6 (Pólya / fixed point):** θ **is** the self-consistent critical point → identify = solve ∂Λ/∂θ′=0, not sample — pays **R7**, **R5**, and **R6-N** (O(10) continuation vs 10³–10⁵ solves) — **wins when** the operator m is known, the regime is monostable, θ is a field θ(x) with unknowns ≫ p, and R5 is trusted.

**Winner: the floor, in A's form** (A is not a speedup over the bare floor — it is the floor stated correctly and hardened at zero cost).

**Eliminations:**
- **C killed — thrice, by arithmetic.** (i) Its efficiency rests partly on a **p-independent** solve, justified against a p≫50 regime the ledger's **p ≈ 50** excludes — §4 itself calls the adjoint's p-independence *"nearly moot."* (ii) Its **O(10)** continuation needs a unique, well-conditioned ∂Λ/∂θ′, but R1 makes that gradient **non-unique in the multistable region** — void exactly where the behaviour is interesting. (iii) Its constraint presupposes R6's *unselected* operator m, while P5 only *"hypothesised"* the critical point. Its own deletion pass concedes it "strips back to the floor."
- **B not killed, but does not win.** Its factoring is internally consistent (the gate acts on input flux, exogenous to θ), but whether the speedup *exists* needs the withheld `C_fwd` breakdown. On current numbers it is floor + precompute + cache-coherence + staleness cost. **Retained as a kill-condition branch**, not a recommendation.

## The commitment (ONE)
**Inference reaches the physics only through `(θ, 𝒫, m) → F` — never through an adjoint, an amplitude equation, a Lyapunov gradient, or the endogenous effective-input field.**

**Kept true by structure:** the simulator is a type whose *sole public method* is `simulate(...) → F`. It exposes **no gradient method and no internal-state accessor**, so a frozen sensitivity, a circular endogenous regressor, or a derivative taken through the non-smooth gate is **unrepresentable** — not merely discouraged by a comment. One decision a reader must internalize; everything else follows.

## Kill question
**Assumption whose falsity would void the design:** "the black box's N-to-attractor appetite fits the budget." **Verdict, from ledger facts only:** unknown — `C_fwd` and `N_eval` are withheld (§4). But the only compliant *alternative*, an adjoint / analytic reduction, is killed by the **same** §4 facts (p-independence moot at p≈50; sensitivity ill-posed near onset). So the assumption cannot be shown false *in favour of a rival*: the honest response is to **measure `C_fwd` and ask the N-budget**, not to switch architecture. **Floor survives.**

## What survives deletion
| Surviving name | Ledger line holding it there |
|---|---|
| Simulator boundary `S` | R7 (endogeneity internal) + R1/R2/R4 (behaviours live inside S) |
| Functional registry `F` (≥2) | R6 (joint constraint pins θ), R3 (over-determined) |
| Bought SBI engine `I` | R6 (calibrated posterior + model verdict; p≈50 native) |
| Post-hoc R5 diagnostic | R5 (tests the hypothesis without coupling the ill-posed gradient into the loop) |
| Extrapolation gate | R6 (score on out-of-sample forcing = extrapolate, not interpolate) |

## What this settles (not built / cannot occur)
- **Not built by default:** the amplitude/Swift–Hohenberg envelope (R1 — S *produces* the pattern; we don't hand-derive it); the adjoint-at-attractor (§4, p≈50); any in-loop Lyapunov-gradient machinery (R5 → diagnostic only); the routing/precompute operator (R2 mechanism → Branch B, off); any regression of θ on the effective-input field (R7).
- **Cannot occur (structurally):** a stale/frozen sensitivity fed to inference; a circular endogenous regressor; a derivative through the rectifier gate or through history-dependent branch selection. All three are unrepresentable behind the boundary — a whole bug-class deleted, not relocated.

## What this makes hard (the price)
- **Sample efficiency** — no gradient acceleration; N integrate-to-attractor solves. *Cope:* sequential SBI/SNPE (~10× fewer N), multi-fidelity coarse-ε surrogates, functional emulation, adiabatic-eliminate w to cut `C_fwd` — **but measure `C_fwd` and ask the N-budget first.**
- **Exploiting R5-if-it-is-a-constraint** — a black box can't natively use self-consistency to slash N. *Cope:* inject R5 as a self-consistent-manifold prior, or flip to Branch C.
- **Near onset / multistable region** — the posterior is broad/multimodal; θ is poorly identified exactly where the behaviour is most interesting (§4). *Cope:* condition on a branch-label functional; report branch multiplicity honestly rather than collapsing it.

## Kill conditions — the map (losing candidates' "wins when" preserved)
- **Activate Branch B (Move 2)** *iff* a measured `C_fwd` breakdown shows the exogenous redistribution ≥ ~20% of `C_fwd` **and** many θ-evals share one forcing scenario **and** no online φ/𝒫 update is required.
- **Flip to Branch C (Move 6)** *iff* R5 is confirmed a **hard** constraint **and** the regime is provably **monostable and** the operator m is known **and** θ must be resolved as a field θ(x) with unknowns ≫ p ≈ 50.
- **Fall back to a local Laplace/adjoint fit** *iff* measured `C_fwd` is so high that even N ~ 10³ busts the budget **and** a usable linearisation exists away from onset.

## Blocking questions to return to the human
1. **`C_fwd`** — cost of one integrate-to-attractor at target resolution. *Decides:* floor feasibility; and (via the exogenous-redistribution fraction) whether Branch B pays.
2. **`N_eval`** — evaluations the inversion demands, i.e. the **required θ-posterior precision + extrapolation range**. *Decides:* floor feasibility; SNPE vs ABC; multi-fidelity need.
3. **R5 — constraint or hypothesis?** *Decides:* R5 enters as a prior/solver (constraint → possibly Branch C) or as a post-hoc diagnostic (hypothesis → floor unchanged).
4. **`d_eff` and the number of independent functionals.** *Decides:* whether R6's joint functionals alone pin θ (`#indep ≥ d_eff`) or the R5 self-consistency closure is load-bearing for identification — i.e. whether question 3 even matters.
5. *(non-blocking, read from data):* ε ratio, threshold Θ, Jensen-gap size, wavelength band, drift speed, #operators. These set functional definitions and simulator config, not the architecture.

## The design
**Parts**
- **`S` — simulator wrapper (the ONE boundary).** Stiff IMEX integrator (ε≪1); inner solve for the adiabatically-slaved fast field w (Fenichel); integrate to the attractor over the slow timescale; non-autonomous forcing carried as a time-dependent scenario. **Public surface: `simulate(θ∈ℝ^p, 𝒫-scenario, m∈{small operator set}) → F` — nothing else.** *Justified:* enforces the commitment by type → R7, and deletes the non-smooth-gate sensitivity bug-class (R1/R2).
- **`F` — functional registry (≥ 2).** Attractor functionals — wavelength band, amplitude, drift speed → R1. Transient functionals — relaxation/return rate, distance-to-threshold, critical slowing → R4. *Justified:* the joint constraint pins θ and licenses extrapolation (R6); over-determined (R3).
- **`I` — inference engine (bought: sequential NPE/SNPE; ABC-SMC fallback).** Per-operator `p(θ|F,m)` + model-selection score over the small set; SBC calibration. *Justified:* R6; p≈50 native, no adjoint (§4).
- **R5 diagnostic (post-hoc, OUTSIDE the loop).** Finite-difference ∂Λ/∂θ′ at the MAP; report `|∂Λ/∂θ′|`. *Justified:* tests R5-as-hypothesis without coupling the ill-posed gradient into inference.
- **Extrapolation gate.** Fit on 𝒫_train; score on 𝒫_test outside the sampled forcing range. *Justified:* R6 (extrapolation, not interpolation).
- **[OFF by default] Branch B amortisation.** `b = D(𝒫,φ,Θ)`, `R_φ = factorize(L_φ)`, reused across θ-evals per scenario. *Justified:* only when kill-condition 1 fires.

**Data flow:** `𝒫-scenario → S(θ,𝒫,m) → F → I → {p(θ|F,m), model score} → extrapolation gate → verdict`; the R5 diagnostic taps θ̂ = MAP off the main path.

**Interfaces:** `S: (θ,𝒫,m) → F ∈ ℝ^{≥2}` (only public method) · `F: state → ℝ^{≥2}` · `I: {F_obs, S, prior} → posterior + model score`. Every part consumes only `F`; none can reach the endogenous field or a derivative through `S`. That is the commitment, made structural.

---

*Process note: produced by the skill's Tier-3 deep search — three isolated proposer contexts under assigned framing moves, adjudicated by a fresh kill-authority judge against an independently sealed scarce-resource derivation. The winner (the floor) matching an `examples.md` ending triggered a re-derivation of scarcity from ledger quantities; it held. The one commitment removes categories of bugs by structure; the reductions the first draft over-committed to survive only as numbered branches with explicit triggers.*
