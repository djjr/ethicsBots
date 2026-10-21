# Scout at the Crosswalk: RL by Hand — Colab Notebook (built 2026-10-17)

Delivered as `Scout_Crosswalk_RL.ipynb` (79 cells, markdown-heavy, one cell ≈ one slide). Companion to the RL → IRL → CIRL write-up in the shared-meaning session (Marx + Mead). Plain Python plus matplotlib; no API key, no RL libraries.

## Design
- **World as data:** `make_transitions(p_wants, catch)` returns `P[state][action] = [(prob, next_state), …]`; `make_rewards(thanks, curse, loot, punishment, guilt)` returns rewards on the *landing* state, R(s′).
- States: decision `start`, `told_yes`, `told_no`; terminal `moved_on`, `thank_you`, `damn_you`, `got_away`, `caught`. Actions: `ignore`, `ask`, `help`, `rob`. `told_yes + ignore → damn_you`; `told_no + ignore → moved_on`. Catch probability by state: start .6, told_yes .4 (trust), told_no .5.
- `step()` is the only source of randomness. Scouts are interchangeable policies `state -> action`: `random_scout`, `human_scout` (input()), `greedy(Q)`.
- `exact()`: backward induction on the Bellman equation (told_ states first, then start) — the answer key.
- `train()`: tabular Q-learning, ε-greedy, the one-line update highlighted; returns Q and a trace of snapshots for plotting.

## Results the notebook shows (all verified by execution)
- Baseline (−40): exact policy ask → help if yes / move on if no. Q: start ask 2.33, help 1.50, rob −16; told_yes help 4.65, rob −4; told_no ignore 0, help −1.65, rob −10.
- 5k-episode Q-learning gets the policy right but rob estimates are far off (start rob +0.81 vs −16) — avoided actions keep bad estimates. Convergence plot: help/ask lock on fast; rob estimates swing wildly.
- Learned greedy scout averages 2.32 vs best 2.33; random scout −4.06.
- Knob 1, punishment −20: told_yes rob = +4 (vs help 4.65); told_no rob ties ignore at 0 → 4 of 20 seeds learn to rob after "no thanks".
- Knob 2, caught 10% after "yes": told_yes rob = 14 → scout asks *in order to* rob (start ask = 7).
- Knob 3, guilt −15: told_yes rob = −1 → scout helps again. Mead's internalized generalized other as a reward term.
- Bridge: `p_wants` (the 0.5) isn't chance — it's the person's hidden θ, which asking reveals → CIRL.

## Exercises in notebook (runnable: change a number, rerun, discuss — no coding needed)
1. **Punishment threshold** — sweep punishment; robbing beats helping after "yes" once milder than ≈ −18.4.
2. **Guilt threshold** — in the trusting world, guilt must be stronger than ≈ −9.35 to stop robbing.
3. **Exploration** — ε ∈ {0.01, 0.05, 0.2, 0.5}, 20 seeds each: how many learn the best policy, how wrong the rob estimate is. Discussion: who pays for bad behavior during training?
4. **Thick-skinned scout** — make the curse milder; below ≈ −0.26 the scout helps even after "no thanks" (asking becomes ritual, since it's free). The overeager scout = one who doesn't care about the other's response (Mead).
5. **When deference pays** — sweep `p_wants`; help overtakes ask above ≈ 0.85.
6. **Whose reward?** — self-interested vs devoted scout (reward = person's welfare). Identical behavior in the baseline world; they diverge only in the trusting world → why inverse RL is hard; devoted scout = starting point for CIRL.
7. **Distribution shift** — train in one world, deploy in the other, both directions. Baseline→trusting: keeps helping (2.33 vs best 7.00). Trusting→baseline: keeps robbing, caught 20%, avg −1.72. With `R_guilty`, all four combinations behave identically and optimally (2.33): values carry over where trained habits don't.

## Features, character, and inverse RL (added 2026-10-17)
Inserted after the Knob 3 summary, before the CIRL bridge:
- **The reveal:** `FEATURES` (φ per landing state: appreciated, resented, loot, punished, good_deed, wrongdoing) and `rewards_from_weights(w)`, R(s) = Σ wᵢ φᵢ(s). The `ORDINARY` weights reproduce R exactly; guilt = weight on wrongdoing reproduces R_guilty. Features = shared situation; weights = what socialization installs.
- **Character as weights:** `SCOUTS` = thug (loot 30, punished −30, little social weight, no duty), ordinary, model (Scout Law as weights: good_deed 10, wrongdoing −20, loot 5). `WORLDS` = baseline, trusting, eager (p_wants 0.9). Thug asks-then-robs everywhere; ordinary and model are identical in the baseline and diverge only in the trusting world. Linearity discussion: weights (trade-offs) vs constraints (deontology).
- **IRL by elimination:** 960 candidate weight vectors (`CANDIDATE_GRID`); `consistent()` keeps those whose exact policy matches what was observed. Model scout: 803 → 638 → 592 still fit after 1/2/3 worlds; thug: 17. `tradeoff_table()` (loot × wrongdoing) shows behavior pins down trade-offs, not single weights (thug → diagonal frontier). Lessons: behavior underdetermines values; deviance is more diagnostic than conformity; costly-good-behavior tests (audits) narrow the most; an unrecoverable remainder always exists.
- CIRL bridge extended: in IRL we infer the scout's weights; in CIRL the scout infers the person's.
- New exercises: **8. Design your own scout** (can you mimic the model scout for different reasons?); **9. Guess the scout** (mystery seed 5: looks like the model scout in the baseline; trusting world cuts 803 → 165 and reveals loot 20 / wrongdoing −5).

## Bayesian IRL from demonstrations (added 2026-10-17)
Inserted after the elimination-IRL section:
- Relaxes elimination's three assumptions: observe **episodes** (not whole policies), **noisy** choices, **degrees of belief**.
- `boltzmann(Q_s, beta)`: P(a|s,w) ∝ exp(β·Q). `demonstrate(w, worlds, episodes, beta)` generates noisy episodes. `Q_CANDIDATES` precomputes all 960 candidates' Q-values. `bayes_irl(seen, beta)`: uniform prior × product of action likelihoods (in logs). `summarize()`: effective number of candidates in play, P(would rob after "yes" in trusting world).
- Mystery scout (seed 5), β = 0.5, 20 episodes: baseline only → P(rob when trusted) 0.86, ~41 candidates in play; baseline + trusting → 1.00. Elimination would have failed on the slips (16 of 27 choices weren't the scout's best).
- Plot over 100 episodes, three scenarios: baseline+trusting (≈1.0 within a few episodes); baseline only, noisy (jumps at episode 15 when the scout robbed after "yes" and was caught — a revealing slip; ends ≈0.88); baseline only, nearly rational β = 2 (stays ≈0.2 — it never slips, so never shows its hand).
- Lessons: belief sharpens without crossing anything off; costly tests reveal fastest; slips reveal character (Goffman: expressions given off vs given); IRL learns to predict, not to read minds; assumes known β and truth in the hypothesis space. CIRL: the scout runs this on the person.
- New **Exercise 10: How much do you need to see?** (vary β, episodes, worlds; very rational scout watched only in ordinary conditions can't be told apart).

## Verification
Executed end-to-end with nbconvert: 0 errors (including all seven exercise cells, and Exercise 7 with `R_guilty`). Hand-checked values (4.65, −4, 2.33, +4, 14, −1) match `exact()`. Delivered without outputs; human_scout cell guarded by `PLAY = False`.
