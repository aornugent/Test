# Requirements

**A spatially extended, multiscale dynamical system: pattern-forming, threshold-forced, structured-reaction transport on a fixed potential — and the inverse problem it poses.**

---

## 1. The system

The object is a **spatially extended dynamical system** on a fixed bounded domain Ω ⊂ ℝ², carrying two coupled fields and a fixed exogenous structure.

**Fixed template.** A time-independent scalar potential φ(x) on Ω, not a state variable. It biases transport but does not evolve. (This is the exogenous-template commitment: the spatial substrate is *given*, not dynamical.)

**Slow field.** A continuum density u(x,t) ≥ 0. Crucially, u is not a primitive scalar with a pointwise reaction term: it is the zeroth moment, u = ∫ n(x,s,t) ds, of an **internal-state-structured density** n over an internal coordinate s. The internal density evolves by a **transport equation in s** — directed advection along s (a drift), a loss (sink) term, and a **nonlocal influx at s = 0** whose magnitude is an integral over n — with rates that depend on the local field w and on a **low-dimensional parameter vector θ ∈ ℝ^p**. So the local dynamics of u is a **nonlocal, state-structured transport operator**, not a scalar reaction term. (Reference family: a linear transport/continuity equation in an internal coordinate with a boundary influx — comparable to kinetic transport in a phase-space coordinate, and to size-distribution kinetics in aerosol and crystal-growth physics.)

**Fast field.** A transported field w(x,t) ≥ 0 governed by a **fast** balance (relaxation parameter ε ≪ 1):

    ε ∂ₜ w  =  input(x,t)  −  depletion(u, w)  +  ∇·[ potential-biased, threshold-gated transport of w ].

The transport moves w down the potential gradient ∇φ and is **gated by a threshold Θ**: redistribution of w engages only where the local input flux exceeds Θ; below it, input stays where it lands. The depletion term is a local sink set by the amount of u present.

**Forcing.** The input is **stochastic and intermittent** — an event process with probability law 𝒫, characterised by a *distribution*, not merely a mean. 𝒫 may be **non-autonomous**: its parameters drift in time.

**Coupling — one channel only.** The two fields close a loop: u raises the local depletion of w (short-range self-enhancement), while the threshold-gated transport **draws w down from the surroundings** of high-u regions (long-range inhibition). There is **no direct nonlocal u–u interaction**: distinct locations of the density field influence one another *only* through the shared field w. (Single-channel commitment.)

**Two timescales.** ε ≪ 1: w relaxes fast, u evolves slowly. On u's slow manifold, w may be **adiabatically eliminated** (slaved). (Reference family: geometric singular perturbation / slow-manifold theory — Tikhonov; Fenichel 1979.)

**Continuum, not particles.** u is a mean-field density — the hydrodynamic / propagation-of-chaos limit of an underlying interacting particle system. Individual realisations and finite-N (fluctuation) noise are **out of scope**. (Reference family: McKean–Vlasov mean-field limits; propagation of chaos — Sznitman 1991.)

---

## 2. Required dynamical properties

### P1 — Finite-wavelength instability of the homogeneous state (non-negotiable)

The spatially uniform steady state (u\*, w\*) must be **linearly unstable to a band of nonzero wavenumbers** k ∈ (k₁, k₂): a symmetry-breaking, finite-wavelength (Turing-type) instability, so that the system's **attractor is a spatially structured state**, not the homogeneous one. The instability must arise **intrinsically from the u–w coupling** — the short-range-activation / long-range-inhibition mechanism above — and **not** be inherited from any spatial structure in φ. On a tilted potential (∇φ ≠ 0) the instability is **convective** and the pattern **drifts** (travelling structure).

The post-bifurcation **branch structure is generically multistable** (subcritical/hysteretic branches are simultaneously stable), so the **selected state is history-dependent**. The apparent variety of stationary morphologies is a single dynamical fact seen from different points on the branch and at different advection strengths. (Reference families: reaction–diffusion–advection systems and the diffusion-driven finite-wavelength (Turing-type) instability, as realised in chemical reaction–diffusion patterns; amplitude/envelope descriptions and the Swift–Hohenberg equation — Swift & Hohenberg 1977; Cross & Hohenberg 1993.)

### P2 — Threshold-gated redistribution; dependence on the forcing *distribution*

The field w is not forced locally; it is **redistributed by potential-biased transport gated by the threshold Θ**. The consequence is the load-bearing one: the **effective local input** is a **nonlinear functional of the forcing law 𝒫**, not of its mean, because the threshold makes the input-to-effective-input map nonlinear — E𝒫[g(input)] ≠ g(E𝒫[input]) (a Jensen gap opened by the gate). Redistribution is therefore also the map that turns a **spatially coarse forcing** into a **spatially resolved effective input** — a downscaling operator fixed by (φ, Θ). The gate is a **non-smooth** operator (a rectifier, max(0,·)-type), which matters for sensitivity (§4). (Reference families: threshold-gated / kinematic transport; shot-noise-driven and piecewise-deterministic dynamics.)

### P3 — Low-dimensional, shared parameterisation of the reaction operator

The structured reaction operator (§1) is controlled by a **shared parameter vector θ of modest dimension** — order p ≈ 50, with **functional interdependencies** that lower the effective dimension. Location-specific quantities (the potential φ, transport/soil coefficients, the local forcing law) are **fixed from the problem data**, not free. Hence θ is *shared across the whole domain*, and the map from θ to the system's behaviour is a **strongly over-determined** one, not an interpolant.

### P4 — Regime generality: attractor *and* transient, autonomous *and* non-autonomous

The system must be analysable **both at its attractor** (the stationary or periodic/drifting structured state) **and along transients**, and specifically **under non-autonomous forcing** (drifting 𝒫), where the system can be **dragged through the P1 bifurcation** by the moving forcing. No analysis may assume autonomy or a fixed attractor. The central non-autonomous question is whether the state **tracks** the slowly moving attractor or undergoes a **rate-induced transition**. The desired transient quantities are **rates**: relaxation/return rates, distance to the bifurcation, and critical slowing down near it. (Reference families: non-autonomous and pullback attractors — Kloeden & Rasmussen 2011; rate-induced tipping — Ashwin, Wieczorek, Vitolo & Cox 2012; fast–slow critical transitions — Kuehn 2011.)

### P5 — A distinguished parameter value as a critical point of a parametric Lyapunov exponent

A distinguished value of θ — or a **θ-field θ(x)** over the domain — is picked out by a **variational condition**, not fitted freely. Define Λ(θ′; θ, A) as the **asymptotic amplification rate (Lyapunov exponent)** of an infinitesimal perturbation carrying shifted parameters θ′, evaluated in the **field configuration (the w-field) produced by the attractor A of the base-θ system**. The distinguished θ satisfies the **vanishing-gradient condition**

    ∂Λ/∂θ′ |_{θ′ = θ}  =  0 ,

a **gradient-flow-to-a-critical-point** condition that is **self-consistent**: the field configuration defining Λ is itself generated by the dynamics at θ. The base case is a **single θ-field varying over Ω**, hypothesised to sit locally at this critical point, so that the spatial variation of θ is *fixed by* the spatially varying effective input of P2. (Reference family: critical points of a parameter-dependent Lyapunov exponent; gradient dynamics on a parameter under a self-consistency constraint — comparable to marginal-stability selection in pattern-forming systems and to self-consistent-field conditions in physics.)

*(Deferred, out of current scope: the branching of the critical point into a multi-branch solution.)*

---

## 3. The inverse problem

Kept deliberately light: the focus here is the system, not the measurements.

**Identification, not prediction.** The task is to **identify θ** and to **select among a small set of candidate governing operators** (competing structures for the coupling and the reaction), matching the system's **attractor and its transients** as read through a set of **state functionals** — left abstract here; "several independent functionals" is all the structure that matters. The requirement is that the **joint** constraint from more than one functional pin θ, and that this joint pinning is what licenses **extrapolation of the identified map beyond the sampled forcing** (rather than interpolation across sampled conditions).

**No tractable likelihood.** The forward map — forcing law 𝒫ₜ ↦ state functionals — runs **through the P1 bifurcation**, with **history-dependent branch selection** and **stochastic forcing**, so it admits **no closed-form likelihood**. Identification and model selection are therefore **likelihood-free**, and the required output is a **distribution over θ** (plus a model-selection verdict), with uncertainty propagated through a **non-smooth, possibly multi-valued** map. (Reference family: simulation-based / likelihood-free inference — as a *problem class*; the choice of machinery is out of scope.)

**The self-consistency identity (the unification).** The covariate that would "explain" θ(x) — the effective-input field of P2 — is **itself a functional of the state** (endogenous: u shapes the redistribution that in turn sets the w it is coupled to). A regression of θ on that covariate is therefore **circular**. The only non-circular constraint is the **self-consistency fixed point**: the configuration in which the θ-field, the attractor, and the w-field are mutually consistent — which is **exactly P5's vanishing-gradient condition evaluated at the attractor**. Thus the "confounding" of the covariate (P2) and the "optimality" of the parameter (P5) are **one and the same fixed-point condition**.

---

## 4. The scarce resource

The binding quantity is the **conditioning of the sensitivity of an attractor-functional to θ**, together with the **cost of the forward map**.

Computing ∂(attractor-functional)/∂θ requires either **tangent propagation** (cost scaling with p) or an **adjoint at the attractor** (cost independent of p but requiring the linearised operator there). The **modest dimension of θ makes the adjoint's p-independence nearly moot** — the usual argument for the adjoint is weak here. And the **P1 bifurcation degrades the conditioning**: the sensitivity of the *selected* state diverges near onset and is **non-unique** in the multistable region, so the sensitivity is ill-posed exactly where the interesting behaviour lives.

The forward map is itself expensive: **integrating a stiff, multiscale (ε ≪ 1), spatially extended system to its attractor over the slow timescale**, repeatedly. The scarce resource is thus

    (cost of one forward integration to the attractor) × (number of forward/sensitivity evaluations the likelihood-free, modest-dimensional, non-smoothly-dependent inversion demands).

Two quantities are left open for the design phase: the cost of one forward integration at target resolution, and the evaluation count the inversion demands.

---

## 5. Generality

None of P1–P5 names what the density u represents. The specification describes **any threshold-forced, fast–slow, structured-reaction transport system on a fixed potential**: an alternative instantiation — a different reaction operator, or a different regime of the parameter vector θ — is *the same problem*. Generality across instantiations is therefore close to free at the level of this specification, because the specification never descends to the identity of the field it governs.

---

## 6. Reference families

*Classes and canonical labels, not application instances.*

- **Pattern formation:** diffusion-driven finite-wavelength (Turing-type) instability, as realised in chemical reaction–diffusion systems; amplitude equations and the Swift–Hohenberg equation (Swift & Hohenberg 1977); the nonequilibrium pattern-formation framework (Cross & Hohenberg 1993); reaction–diffusion–advection instabilities.
- **Structured transport:** a linear transport/continuity equation in an internal coordinate with a boundary influx — comparable to kinetic transport in a phase-space coordinate, and to size-distribution kinetics in aerosol and crystal-growth physics (advection in a size coordinate with a nucleation influx).
- **Mean-field limits:** McKean–Vlasov dynamics; propagation of chaos (Sznitman 1991).
- **Multiscale reduction:** geometric singular perturbation and slow manifolds (Tikhonov; Fenichel 1979); adiabatic elimination / slaving.
- **Threshold / intermittent forcing:** threshold-gated transport; shot-noise-driven and piecewise-deterministic Markov dynamics.
- **Distinguished parameter as a critical point:** critical points of a parameter-dependent Lyapunov exponent; gradient dynamics on a parameter under a self-consistency constraint — comparable to marginal-stability selection and self-consistent-field conditions in physics.
- **Non-autonomous dynamics and tipping:** pullback / non-autonomous attractors (Kloeden & Rasmussen 2011); rate-induced tipping (Ashwin et al. 2012); fast–slow critical transitions (Kuehn 2011).
- **Inverse problem:** likelihood-free / simulation-based identification and model selection (as a problem class).
