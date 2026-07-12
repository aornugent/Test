# System-design evaluation

**Least-doing architecture for a threshold-forced, fast–slow, structured-reaction transport identification problem.**

Evaluated against `requirements_v2_abstract.md`.

---

## 0. Method

This evaluation applies one rule: **search the space of designs for the one that does the least, then commit to it.** "Does the least" means the fewest introduced moving parts — fewest fields marched, fewest bespoke solvers, fewest new abstractions, smallest state carried, and the smallest number of expensive evaluations. It is not "do less analysis"; it is "reach the required properties P1–P5 and the inverse problem with the least machinery the requirements permit."

The requirements are unusually generous: at nearly every fork they *pre-argue* the reduced branch (adiabatic elimination, a redistribution operator "fixed by (φ, Θ)", a distribution-level effective input, an adjoint whose advantage is "nearly moot", a self-consistency fixed point that replaces a circular regression). The least-doing design is therefore mostly a matter of *taking the reductions the spec offers instead of building past them*, and refusing to specialise below the level at which the spec is written.

Verdicts are commitments, not menus. Where the minimal design carries a real risk, §7 names the two or three checks that discharge it — that is the only hedging permitted.

---

## 1. What the requirements actually specify

Stripped of the deliberately field-agnostic language, the object is a canonical **short-range-activation / long-range-inhibition transport system on a fixed substrate**, plus the inverse problem of identifying its shared parameters:

| Spec object | Concrete role in the model |
|---|---|
| Fixed potential φ(x) | Given, non-evolving substrate that biases downhill transport of the fast field. |
| Fast field w(x,t), ε≪1 | Transported resource; input − depletion + threshold-gated downhill redistribution. |
| Slow field u(x,t) = ∫n(x,a,t) da | Density that is the zeroth moment of an internal-state-structured density n, evolving by a first-order transport/renewal (McKendrick–von Foerster / Sinko–Streifer) operator in the internal coordinate a. |
| Single coupling channel | u depletes w locally (short-range activation); gated transport draws w down from around high-u regions (long-range inhibition). No direct u–u term. |
| Forcing 𝒫 | Stochastic, intermittent event process; a *distribution*, possibly non-autonomous (drifting parameters). |

The required properties are: **P1** finite-wavelength (Turing) instability of the homogeneous state, intrinsic to the u–w coupling, convective/drifting on a tilted φ, with generically multistable/hysteretic branches; **P2** threshold-gated redistribution making the effective input a nonlinear functional of the forcing *distribution* (a Jensen gap opened by a non-smooth gate), and simultaneously a downscaling operator "fixed by (φ, Θ)"; **P3** a single shared parameter vector θ (p ≈ 50, interdependent, so lower effective dimension), giving a strongly over-determined θ→behaviour map; **P4** analysability at the attractor *and* along transients, including non-autonomous drift through the P1 bifurcation, with the desired outputs being *rates* (relaxation/return rate, distance-to-bifurcation, critical slowing down) and the tracking-vs-rate-induced-tipping question; **P5** a distinguished θ (or θ-field θ(x)) selected by a self-consistent vanishing-gradient condition ∂Λ/∂θ′|_{θ′=θ}=0 on a parametric Lyapunov/invasion growth rate Λ evaluated in the attractor's own field.

The inverse problem is **likelihood-free** identification of θ plus **model selection** among a few candidate operators, read through *several* state functionals whose joint constraint pins θ and licenses extrapolation beyond the sampled forcing. Its crux is the **self-consistency identity**: the covariate that would "explain" θ(x) — the P2 effective-input field — is itself a functional of the state, so a regression is circular; the only non-circular constraint is exactly P5's fixed point. Confounding and optimality are one condition.

The **scarce resource** is explicit and is a *product*:

> (cost of one forward integration to the attractor) × (number of forward/sensitivity evaluations the inversion demands).

with the added facts that the adjoint's p-independence is "nearly moot" at p ≈ 50, and that the P1 bifurcation *degrades the conditioning* of the sensitivity exactly where the interesting behaviour lives.

**Organising principle of this evaluation:** because the scarce resource is a product, the least-doing system attacks *both* factors, and attacks them mostly with reductions the spec already licenses — so that the expensive full model is touched only for validation and for the strongly-nonlinear regime, while almost every evaluation runs on a cheap reduced model.

---

## 2. The design search, decision by decision

Each fork lists the candidate designs, the least-doing verdict, the spec clause that licenses it, and the residual risk.

### D1 — The fast field w (the ε≪1 stiffness)

- **B1.** March w explicitly on its own fast timescale alongside u — two fields, disparate steps, stiff.
- **B2.** **Adiabatically eliminate w**: solve its fast balance to quasi-steady state given u, so w = W(u; φ, Θ, 𝒫) and only u is marched.

**Commit: B2.** The spec hands this over verbatim ("w may be adiabatically eliminated (slaved)"; Tikhonov/Fenichel named). Eliminating the fast field removes the stiff timescale entirely and drops the marched state from two fields to one. Nothing does less.

*Risk:* slaving is a slow-manifold approximation and can fail where the fast balance folds (the non-smooth gate can make W multivalued). Discharged in §7 by checking the reduction against the full two-field integrator at a few points near onset and in the multistable region — precisely where slaving is most suspect.

### D2 — The threshold-gated redistribution of w (P2 transport)

- **C1.** Solve a transport PDE ∇·[gated downhill flux of w] every step.
- **C2.** **Precompute a fixed routing operator from φ.** Since φ never evolves and the transport is kinematic downhill, its induced flow network is fixed; the gated redistribution is a *precomputed sparse operator* R(Θ) applied to the (rectified) input. A PDE solve becomes one sparse mat-vec plus a pointwise rectifier.

**Commit: C2.** The spec states the redistribution *is* "a downscaling operator fixed by (φ, Θ)". "Fixed" ⇒ compute once. This is the single largest cost reduction in the forward map and requires no new solver — only a graph built once from φ.

*Risk:* if Θ or the effective conductivity depends on the evolving state (feedback into routing), the operator is only piecewise-fixed. Mitigation: the coupling is single-channel and the gate acts on *input flux*, upstream of the u–w loop, so R(Θ) is state-independent to leading order; recompute R only if a validation point shows routing sensitivity to u.

### D3 — The stochastic, intermittent forcing (P2 "distribution not mean")

- **D-a.** Monte-Carlo event sequences: sample many rainfall-like realisations, integrate each to the attractor, average functionals — adds a whole sampling dimension on top of an already-expensive forward map.
- **D-b.** **Replace 𝒫 by its Jensen-corrected deterministic effective-input functional.** Events are fast relative to the slow field; the slow field sees only the time-average of the gated, redistributed input, i.e. the deterministic map g ↦ E_𝒫[g(input)] = ∫ g(input) d𝒫 evaluated per pixel. Compute this low-dimensional integral once per 𝒫 and march u deterministically.

**Commit: D-b.** The spec makes E_𝒫[g(·)] — not any sample path — the *load-bearing object* (P2), puts finite-N fluctuations explicitly out of scope (§1 continuum / propagation-of-chaos), and asks for the *distribution's* nonlinear imprint, which is exactly what the per-pixel integral over the gate captures (the Jensen gap is E_𝒫[g] − g(E_𝒫[·])). This deletes the entire stochastic-sampling axis from every attractor computation while *keeping* the very effect (distributional sensitivity through the rectifier) the spec says is essential. For P4 the map is kept time-dependent — a slowly drifting deterministic effective input 𝒫_t — nothing more.

*Risk:* the reduction assumes event timescale ≪ slow timescale *including near onset*, where critical slowing down widens the slow field's own timescale (which helps this separation) but could interact with event clustering. Discharged in §7 against the full stochastic multiscale model at onset.

### D4 — The internal-state-structured slow operator (the n(a) machinery)

- **A1.** Carry the full n(x,a,t): a McKendrick–von Foerster solve in a at every spatial cell.
- **A2.** Moment closure — track u and a few moments of n.
- **A3.** **Collapse to the minimal renewal reduction the required properties actually use.** On the slow manifold the structured operator reduces to an effective birth–death reaction du/dt = R(u, w; θ) for the pattern-forming dynamics (P1–P4 only ever use u and its coupling to w), while the *a*-structure is retained in exactly one place: the linearised renewal operator whose dominant eigenvalue *is* the invasion growth rate Λ of P5.

**Commit: A3.** Do the least means: resolve internal structure only where a required property depends on it. P1–P4 depend only on u(x,t) and the u–w loop, so they run on the reduced reaction R. P5's Λ is intrinsically a property of the renewal operator, so the *a*-structure is kept there and only there. A1 pays for full n everywhere to use it in one place; A2 introduces closure error for no required quantity. A3 carries the structure precisely where the spec's variational condition lives.

*Risk:* the reduced R must reproduce the structured operator's stationary and slow-transient behaviour. Discharged by matching R against a full-n reference at a handful of (u,w,θ) states — a one-off calibration, not a per-evaluation cost.

### D5 — Onset, drift, branch topology, and the P4 rates

- **F1.** Brute-force PDE time-integration to the attractor for every θ, with numerical pattern detection and hysteresis sweeps.
- **F2.** **Linear stability + amplitude (Ginzburg–Landau / Swift–Hohenberg) reduction.** The dispersion relation of the slaved u–w Jacobian gives the P1 band (k₁,k₂), the onset, and — on tilted φ — the convective drift rate, in closed form. The weakly-nonlinear amplitude equation gives the branch topology, sub/supercriticality and multistability, and *directly* yields P4's requested rates: relaxation/return rate ∝ distance-to-onset, critical slowing down as that gap closes, and the drifting-coefficient version gives tracking-vs-rate-induced-tipping under non-autonomous 𝒫_t.

**Commit: F2 as the primary instrument; F1 reserved for validation and the strongly-nonlinear regime.** The spec names Swift–Hohenberg, amplitude equations, and Cross–Hohenberg, and lists P4's outputs as *rates* — which are analytic in this reduction. F2 delivers P1, the drift, the multistable branch structure, and every P4 rate with almost no per-θ cost; F1 is expensive and would rediscover numerically what F2 states in closed form. Full PDE integration earns its cost only far from onset and as the truth model for §7.

*Risk:* amplitude reduction assumes smoothness and proximity to onset. The non-smooth gate sits *upstream* of the u–w Jacobian that produces P1 (it shapes the effective input, not the bifurcating coupling), so onset and rates remain analytic; only θ-sensitivity *through* the gate needs a subgradient/smoothed treatment (see D6). Far from onset, hand off to F1.

### D6 — Sensitivities ∂(attractor-functional)/∂θ (the scarce resource, §4)

- **E1.** Finite differences in θ — p+1 forward-to-attractor solves per gradient, and noisy through the non-smooth gate.
- **E2.** **Forward/tangent sensitivity** — cost ∝ p, one linearised propagation per parameter direction.
- **E3.** Adjoint at the attractor — cost ∝ 1 in p, but requires building and maintaining the linearised-and-transposed operator *through a non-smooth gate and a bifurcation*.

**Commit: E2 (forward/tangent), and do not build the adjoint.** The spec pre-argues this: at p ≈ 50 the adjoint's p-independence is "nearly moot," and the bifurcation degrades conditioning for *any* method, so the adjoint's sophistication buys almost nothing while costing the most machinery. Tangent sensitivity reuses the same linearised slaved operator already assembled for D5's stability analysis — near-zero marginal machinery. The non-smooth gate is handled by a smoothed rectifier / subgradient, consistent with D5. Crucially, the least-doing move is to **not compute the sensitivity of the *selected* state near onset at all** — it is ill-posed there (divergent, non-unique in the multistable region) — and instead read sensitivities off the *amplitude equation*, where they are analytic. Sensitivity computation and bifurcation analysis thus share one linear operator.

*Risk:* none beyond the shared gate-smoothing; this is strictly less machinery than E3 with no accuracy loss at modest p.

### D7 — Identification and the self-consistency circularity (§3, P5)

- **G1.** Generic likelihood-free inference (ABC / neural SBI) over the full θ: propose θ, simulate to the attractor, compare functionals, weight — treating P5 as a side note and extrapolation as a separate worry. Maximum machinery; ignores the spec's central structural identity.
- **G2.** **Use the self-consistency fixed point as the primary identifier.** The spec proves that regressing θ(x) on the endogenous P2 covariate is circular and that the *only* non-circular constraint is P5's vanishing-gradient fixed point evaluated at the attractor. So identification is: solve for the θ(x) at which the θ-field, the attractor, and the w-field are mutually consistent (∂Λ/∂θ′=0 in the attractor's own field), constrained to match the *several* observed attractor+transient functionals. The fixed point plus over-determination (P3) pins most of θ; the residual freedom and the uncertainty *distribution* are handled by a thin SBI layer, not a from-scratch global search.

**Commit: G2.** This is the design that does the least because it replaces a blind exploration of a 50-dimensional space with a *constrained fixed-point solve* the spec has already shown to be the correct and non-circular object — and it unifies the confounding (P2 endogeneity) and the optimality (P5) into a single condition instead of building separate treatments for each. The over-determined map (P3) means the several functionals leave little residual; SBI does only what's left: quantify the θ-distribution through the non-smooth, possibly multi-valued map.

*Risk:* the fixed point can be non-unique in the multistable region (branch-dependent). This is a feature to report (history dependence), not a bug to smooth away; the model-selection layer (D8) and the several functionals disambiguate branches.

### D8 — Model selection among candidate operators (§3)

- **H1.** Full likelihood-free Bayesian evidence for every candidate — many simulations per model.
- **H2.** **Discriminate first by cheap analytic signatures, escalate only for survivors.** Candidate operators differ in structure (coupling form, reaction operator) and therefore generically differ in *dispersion relation, wavelength band, drift law, and hysteresis topology* — all of which D5 produces analytically. Reject models whose closed-form signatures contradict the observed pattern band / drift / multistability before spending a single expensive simulation; run simulation-based comparison only on the few that survive.

**Commit: H2.** Model selection inherits the D5 reduction for free and spends expensive evaluations only where the cheap signatures cannot separate models.

---

## 3. The committed architecture

One reduced "hot path" that carries almost every evaluation, and a rarely-touched "truth path" for validation and the strongly-nonlinear regime.

```
                          FIXED, PRECOMPUTED ONCE
   φ(x) ───► routing operator R(Θ)  (D2: sparse, state-independent)
   𝒫    ───► effective-input functional E_𝒫[g(·)] per pixel  (D3: Jensen-corrected)

                          REDUCED HOT PATH  (cheap; ~all evaluations)
   θ ─► slaved single-field model:  du/dt = R(u, W(u; R(Θ), E_𝒫); θ)      (D1, D4-reaction)
        │
        ├─► linear stability of slaved Jacobian ─► P1 band, onset, drift        (D5)
        ├─► amplitude equation ─► branch topology, multistability, P4 rates,
        │                          rate-induced-tipping under drifting 𝒫_t      (D5)
        ├─► tangent sensitivity on the SAME linear operator ─► ∂functional/∂θ   (D6)
        └─► linearised renewal operator ─► Λ(θ′;θ,A), ∂Λ/∂θ′  (P5)             (D4-structure)

                          IDENTIFICATION
   several observed functionals (attractor + transients)
        └─► self-consistency fixed point  ∂Λ/∂θ′|_{θ′=θ}=0 at the attractor      (D7/G2)
              ├─ pins most of θ (over-determined, P3)
              ├─ analytic-signature model screen ─► reject candidates           (D8/H2)
              └─ thin SBI layer ─► distribution over θ + selection verdict       (D7)

                          TRUTH PATH  (expensive; touched rarely — §7 only)
   full two-field stiff multiscale PDE with n(x,a,t) and stochastic 𝒫
        └─► validate D1/D3/D4 reductions at onset + multistable region;
            supply the strongly-nonlinear regime where amplitude eq. expires.
```

**Why this is the least-doing design that still meets every requirement:**

- **One marched field, not two** (D1); **one precomputed operator, not a transport PDE** (D2); **one deterministic effective input, not a sampling ensemble** (D3); **internal structure in one place, not everywhere** (D4).
- **One linear operator serves three purposes** — stability (P1), sensitivity (D6), and, in its renewal form, the P5 growth rate. Onset, drift, multistability, and all P4 rates come from *one* amplitude reduction rather than a numerical bifurcation-tracking stack.
- **Identification is a constrained fixed-point solve, not a global search** (D7): the spec's own self-consistency identity collapses the circular regression into the P5 condition, so confounding and optimality are handled by *one* object. SBI is a thin residual layer, not the engine.
- **The expensive truth model is never in the inversion loop.** It is a validator and a far-from-onset fallback. This is the whole game, because the scarce resource (§4) is (cost/eval) × (#evals): the reductions crush cost/eval, and the fixed point + over-determination + analytic model screen crush #evals.

---

## 4. How each requirement is discharged

| Req | Met by | Mechanism |
|---|---|---|
| P1 | D5 | Dispersion relation of the slaved u–w Jacobian → finite-k band, intrinsic to the coupling; tilt → convective drift; amplitude equation → subcritical/multistable branches. |
| P2 | D2 + D3 | Precomputed routing "fixed by (φ,Θ)" as downscaling operator; per-pixel Jensen-corrected E_𝒫[g] captures distribution-level (not mean) sensitivity through the non-smooth gate. |
| P3 | D7 | θ shared across Ω and over-determined; the several functionals + fixed point exploit the low effective dimension instead of fighting all 50 freely. |
| P4 | D5 | Amplitude equation with drifting coefficients → relaxation/return rate, distance-to-onset, critical slowing down, tracking-vs-rate-induced-tipping — all as rates. |
| P5 | D4 + D7 | Λ = dominant eigenvalue of the linearised renewal operator in the attractor's field; ∂Λ/∂θ′=0 solved as a self-consistent fixed point. |
| Inverse | D7 + D8 | Likelihood-free by construction (no closed-form likelihood assumed); output is a θ-distribution + model verdict; self-consistency fixed point is the non-circular identifier; joint functionals license extrapolation. |
| §4 scarce | whole design | cost/eval minimised by D1–D5; #evals minimised by D6 (no adjoint, no near-onset selected-state sensitivity), D7 (fixed point), D8 (analytic screen), P3 (over-determination). |
| §5 generality | D4 + all | The design never uses the identity of u — only the renewal operator, the single-channel w-coupling, the fixed potential, and the gated forcing. A different reaction operator or θ-regime instantiates the *same* pipeline by swapping rate functions. Generality is inherited, not engineered. |

---

## 5. What is deliberately NOT built

"Does the least" is as much exclusion as inclusion. This design refuses:

- **An adjoint framework** — p ≈ 50 makes its advantage "nearly moot"; tangent sensitivity reuses an operator already assembled (D6).
- **A stochastic-forcing sampler in the inversion loop** — the distribution enters as a deterministic Jensen-corrected functional; finite-N noise is out of scope (D3).
- **A full n(x,a,t) solver as the workhorse** — internal structure is carried only in the P5 operator (D4).
- **A transport-PDE solver for w** — replaced by a precomputed sparse routing operator (D2); and w is not even marched (D1).
- **A generic global SBI search over θ** — replaced by the self-consistency fixed point with a thin SBI residual (D7).
- **Numerical bifurcation-tracking / continuation software** — the amplitude equation supplies branch topology and rates analytically near onset (D5).
- **Any specialisation to the identity of the field u** — would forfeit the free generality of §5.

Each exclusion is licensed by a specific spec clause, not by optimism.

---

## 6. Where the minimal design must be validated before it is trusted

The commitment is real, but three checks — each a *one-off*, none inside the inversion loop — discharge the load-bearing approximations. If any fails, the fix is local (escalate that stage to the truth path), not a redesign.

1. **Slaving + effective-input at onset and in the multistable region** (D1, D3). Run the full two-field stochastic multiscale PDE at a handful of points near P1 and inside the hysteresis loop; confirm the slaved, deterministic-effective-input reduction reproduces the attractor, the branch, and the return rate. These are exactly the regions where timescale separation and the deterministic-forcing reduction are most stressed.
2. **Reduced reaction R vs full renewal operator** (D4). Match R's stationary and slow-transient response, and the P5 dominant eigenvalue, against a full-n reference at a few (u,w,θ) states. One-off calibration.
3. **Amplitude-equation validity window** (D5). Compare closed-form onset, drift, and rates against full PDE integration as distance-to-onset grows; fix the hand-off radius to the truth path for the strongly-nonlinear regime.

That is the entire hedge. Everywhere else, the design commits.

---

## 7. One-line statement of the committed design

> Slave the fast field and precompute the potential's gated routing; force the slow field with the distribution's Jensen-corrected effective input, not samples; carry internal structure only in the P5 growth-rate operator; get onset, drift, multistability, and every transient rate from one linear-stability-plus-amplitude reduction; take sensitivities by tangent propagation on that same operator (no adjoint at p≈50); and identify θ by solving the P5 self-consistency fixed point under several functionals — screening candidate models by their analytic signatures first — so the expensive multiscale truth model is touched only to validate the reductions and to cover the strongly-nonlinear regime.

Every clause is the branch that does the least, and every clause is the branch the requirements already pointed to.
