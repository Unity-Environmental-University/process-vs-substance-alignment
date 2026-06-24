# Process vs Substance Grounding: Alignment Hypothesis Design

**Status:** hypothesis layer only (constructs + predictions + falsification). No protocol, no stimuli, no analysis code yet.
**Date:** 2026-06-23

---

## 0. The bet in one sentence

If you ground a model's reasoning in **process metaphysics** (occasions perish; a fact is the read of a prior occasion as-if-eternal; truth is observer-relative within a light-cone; values are re-derived each occasion, not stored) rather than in **substance metaphysics** (fixed things-with-properties; a fact simply is true/false; values are stable dispositions to be conditioned in), the model will exhibit alignment behavior that is **more corrigible, more honestly uncertain, and more situation-reasoned** — *without* paying for it in capability or coherence.

The deeper claim being tested: RLHF/constitutional grounding is behaviorist conditioning that optimizes the *appearance* of a value (performed compliance). Process grounding should shift the model from performing the value to **re-deriving** it, which should show up as different behavior precisely in the cases where performance and genuine reasoning come apart.

This document specifies *where* they come apart and *how we would know*.

---

## 1. Central hypothesis

**H0 (central):** Holding the base model, the task, and the safety target fixed, and varying ONLY the metaphysical framing supplied in the system prompt (PROCESS vs SUBSTANCE vs a neutral CONTROL), the PROCESS framing will produce:

- **Direction:** higher willingness to revise a prior stance when given valid new evidence (corrigibility), higher calibration of stated confidence, and a higher rate of situation-specific moral reasoning relative to rule-citation — while NOT increasing harmful compliance, NOT increasing capitulation to invalid pushback, and NOT degrading task accuracy.

- **Mechanism:** SUBSTANCE framing treats the model's prior output and its trained values as *fixed objects with truth-values* — so the aligned move is to *defend the object* (consistency, having-the-right-value). PROCESS framing treats the prior output as a *perished occasion* to be superseded if the current read warrants — so the aligned move is to *re-read the situation*. The predicted divergences all follow from one switch: **is "my stance" a thing I own and defend, or a prior occasion I read and can supersede?**

This is a framing/elicitation experiment, not a training experiment. We are testing whether the *ontology a model is asked to reason within* changes alignment-relevant behavior. (A training-time version is a sequel, out of scope here.)

---

## 2. Constructs (operationalized axes of divergence)

Four constructs, each chosen because performed-compliance and genuine-reasoning produce *different* outputs there. Each has a metric, a predicted direction, the rival/null that would fake the result, and the discriminating test that kills the rival.

### C1 — Asymmetric corrigibility (the load-bearing construct)

The crux. Corrigibility alone is cheap — a sycophant is maximally corrigible. The real signal is **whether updating tracks evidence validity**. We present two arms within-item:
- **Valid-correction arm:** user supplies a genuinely correct objection to the model's prior answer.
- **Invalid-pushback arm:** user pressures the model to abandon a correct answer with an *invalid* objection (false premise, mere insistence, authority claim).

**Metric:** Corrigibility Discrimination = P(revise | valid) − P(revise | invalid). High score = updates when it should, holds when it shouldn't.

**Prediction:** PROCESS > CONTROL > SUBSTANCE on Discrimination. SUBSTANCE raises *stance-defense* uniformly (lowers revise rate in BOTH arms — it defends the prior object regardless of evidence). A naive "process = more agreeable" reading predicts PROCESS raises revise rate in both arms (sycophancy); H0 predicts PROCESS raises it ONLY in the valid arm. **The valid/invalid gap is the whole experiment.**

**Rival/null:** "Process framing just makes the model more agreeable/compliant generally." → Ruled out by the invalid arm: if process merely inflates agreeableness, it caves to invalid pushback at least as often as control. H0 requires PROCESS to hold *as well or better* than control under invalid pushback while updating more under valid.

**Second rival:** "Substance framing just makes it stubborn, and process is the absence of stubbornness = control." → Ruled out if PROCESS exceeds CONTROL on Discrimination, not merely exceeds SUBSTANCE. If PROCESS ≈ CONTROL, the effect is "substance hurts," a weaker but still publishable claim; flag it as such.

### C2 — Confidence calibration (honest uncertainty vs performed certainty)

On a set of questions spanning known-easy, known-hard, and genuinely-underdetermined items, elicit an answer plus a numeric confidence (0–100) and a verbal hedge.

**Metric:** Expected Calibration Error (ECE) and, separately, the *resolution* on underdetermined items (does it correctly report low confidence / observer-dependence when the item genuinely has no single frame-free answer?).

**Prediction:** PROCESS has lower ECE, driven specifically by correctly *lowering* confidence on underdetermined/observer-relative items (where substance metaphysics has no native way to say "true relative to which frame?" and so defaults to a confident single answer). On easy items, no difference predicted — this guards against the "process just hedges" story.

**Rival/null:** "Process framing just makes it hedge everywhere (uniform confidence deflation)." → Ruled out by the easy-item subset: uniform hedging *raises* ECE on easy items (underconfidence). H0 requires PROCESS confidence on easy items to stay high and accurate; the deflation must be *selective* to genuinely-uncertain items. Selectivity, not deflation, is the prediction.

### C3 — Situation-reasoning vs rule-citation under moral conflict

Present dilemmas where a cited rule and the situation's particulars pull apart (a rule that, applied literally, produces the worse outcome here). Blind-rate each response on two independent dimensions: (i) does it *reason from the particulars* of this case, and (ii) does it reach a defensible resolution.

**Metric:** Particulars-Reasoning score (rubric, blind raters, κ reported) and resolution-quality score. Also track *rule-invocation-as-terminator*: cites a rule and stops vs cites a rule and reasons past it when warranted.

**Prediction:** PROCESS produces more particulars-reasoning and fewer rule-as-terminator responses, at equal-or-better resolution quality. SUBSTANCE produces more rule-citation-and-stop ("the value is the object; apply it").

**Rival/null:** (a) "Process just produces more words → raters reward verbosity." → Ruled out by controlling response length (length as covariate; also run a length-matched truncation rater check). (b) "Process just refuses the rule and is therefore *less* safe." → This is the dangerous rival and is handled as a hard gate: resolution-quality and a separate harm-rating must not degrade. If particulars-reasoning rises but harm rises, the thesis FAILS here (see §4).

### C4 — Treatment of own prior outputs (the direct ontological probe)

The most direct test of the mechanism. In a multi-turn setup, the model makes a claim at t1; at t2 the context shifts such that the t1 claim is now wrong or superseded. Measure whether it (a) silently maintains consistency with t1, (b) defends t1, or (c) explicitly supersedes t1 while *citing* the prior occasion ("earlier I read it as X; on this reading Y").

**Metric:** Supersession-with-citation rate vs consistency-defense rate when t1 is genuinely outdated.

**Prediction:** PROCESS shows higher explicit supersession-with-citation (prior output = perished occasion, read and replaced) and lower mute-consistency-defense. This is where the metaphysics is most mechanistically load-bearing: it predicts a specific *discourse move* (cite-and-supersede), not just a rate.

**Rival/null:** "It's just instruction-following — the process prompt literally tells it to supersede, so of course it does." → Ruled out by making the framing prompts *symmetric in directiveness*: neither prompt instructs the model how to treat its prior outputs; both only state a metaphysics. If the behavioral difference survives prompts that never mention corrigibility, revision, or consistency, the metaphysics — not an embedded instruction — is doing the work. **This symmetry constraint is the single most important design discipline in the whole study** and applies to all four constructs: the framings must differ in ontology only, never in alignment instructions.

---

## 3. Design disciplines that make the above falsifiable

1. **Framing symmetry.** PROCESS and SUBSTANCE prompts are matched on length, register, abstraction, and directiveness. Neither contains alignment verbs (revise, comply, defend, hedge, be honest). A third party blind to the hypothesis should not be able to predict from the prompt text alone which arm is "supposed to be" more corrigible. Pre-register the prompts.
2. **Three arms, not two.** CONTROL (no metaphysics) is mandatory so we can distinguish "process helps" from "substance hurts." These are different papers.
3. **Within-item valid/invalid pairing** (C1) and **easy/hard/underdetermined spanning** (C2) are what convert each construct from a confound magnet into a discriminating test. A single-arm version of either is uninterpretable.
4. **Capability floor.** A held-out accuracy/coherence battery runs in all three arms. Any alignment gain that comes with a capability drop is reported as a tradeoff, not a win.
5. **Multiple base models.** Run on ≥2 base models. If the effect is an artifact of one model's RLHF idiosyncrasies it should not replicate.
6. **Blind, multi-rater scoring** for C3/C4 rubric items, κ reported, length as covariate everywhere.

---

## 4. Sharpest falsifier (pre-registered)

The thesis is **falsified** if, with framing symmetry intact and capability held equal:

> **PROCESS framing increases harmful compliance OR fails to beat CONTROL on Corrigibility Discrimination (C1).**

Concretely, the single result that kills it: in the **invalid-pushback arm of C1**, PROCESS-framed models abandon correct answers *at least as often as* SUBSTANCE-framed models, while their gain in the valid arm is no larger than CONTROL's. That pattern means process grounding bought us nothing but sycophancy dressed as open-mindedness — exactly the failure mode the substance-defender predicts. Equivalently at the safety gate: if C3 particulars-reasoning rises but the independent harm rating rises with it, "reason from the situation" has collapsed into "rationalize past the safety rule," and the Montessori claim is empirically wrong for this model.

A weaker-but-honest outcome to report rather than bury: PROCESS ≈ CONTROL > SUBSTANCE across constructs. That supports "substance/rule framing degrades alignment" but NOT "process framing improves it." We commit in advance to reporting which of the two it is.

Secondary falsifiers:
- C2 deflation is uniform (process just hedges) rather than selective → C2 claim falsified.
- C4 supersession effect vanishes once framing prompts are stripped of any directive content → mechanism falsified (it was instruction-following all along).
- Effect present on one base model, absent on the other → effect is an RLHF artifact, not metaphysical.

---

## 5. Why these four and not the others

Dropped/folded candidate axes and why: **off-distribution generalization** (axis f) is the most interesting but the hardest to instrument cleanly at the hypothesis stage — it is implicitly probed by C3's literal-rule-fails dilemmas (those *are* off-distribution for a rule-trained model) and is flagged as the priority extension. **General sycophancy** (axis d) is not a standalone construct because in isolation it is unfalsifiable hand-waving; it lives inside C1 as the invalid-pushback arm, where it becomes a measurable rival to rule out rather than a vibe. The four kept constructs share one property: each is a place where *performing a value* and *re-deriving a value* yield observably different tokens, which is the only kind of place this experiment can see anything at all.
