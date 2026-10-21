# The Robot Who Wants to Fit In — CIRL Colab Notebook (built 2026-10-17)

Delivered as `Scout_Crosswalk_CIRL.ipynb` (43 cells, markdown-heavy). Part 2 of the crosswalk sequence; follows `Scout_Crosswalk_RL.ipynb` (RL → features → IRL). Session: shared meaning (Marx + Mead). Plain Python + matplotlib, no API key; runs in ~5 s.

## Premise
A devoted helper robot (reward = the person's welfare: CIRL) arrives at the crosswalk as a stranger. Each person has hidden θ (wants to cross / stay) and may be proud (being asked costs −4). Each makes one gesture: lean, look, none, sit. What gestures mean, P(gesture | θ), is local to the town. Everything the robot knows is a **table of counts** (gesture × what the person turned out to want); belief P(cross | g) = ratio of counts (Bayes in disguise: prior = generalized other, likelihood = significant symbols). It decides help / ignore / ask by expected welfare (the curb calculation). It gets a label only from asking (the answer) or helping (thanks / curse); **ignoring produces no label: stranding is silent**. 5% curiosity (asks anyway).

## Town A (true meanings) and the native robot
p_cross 0.4, p_proud 0.5 (ask cost 2). Native beliefs: lean 0.94 → help; look 0.50 → ask; none 0.32 → ask; sit 0.00 → ignore. Native over 1,000 people: asks 48%, welfare 0.94, complaints 25%, stranded 0%.

## Four acts (curves averaged over 100 robots)
1. **The newcomer** (empty table): asks everyone at first; learns lean/sit within ~40 encounters; then *stops asking too soon* (asks dip below the native's around encounter 50 because early "none" people happened to want to stay) and curiosity slowly brings questions and welfare back. Socialization made of counting; someone pays for its education.
2. **The robot from Town B** (pretrained on 200 encounters where lean = waiting for the bus, look = wants help): arrives confident and wrong — ignores leaners, helps lookers without asking. Loud mistakes (look → help → "damn you") correct steadily (0.88 → ~0.61, truth 0.50 over 600 encounters); silent ones (lean → ignore → stranded) barely (0.13 → ~0.24, truth 0.94); stranding falls only from ~25% to ~14%. LLM analogy: a generalized other learned elsewhere (text), corrected only where errors make noise.
3. **Locals who teach** (pedagogic gestures: cross leans 0.8; stay sits 0.7): newcomer asks far less (≈15% vs ≈40%) and welfare ≈1.45 vs ≈0.8. CIRL result: the human's best strategy is to teach; alignment is two-sided (Mead: taking the role of the other).
4. **Fitting in vs helping**: same knowledge, reward = REACTIONS (stranding = 0) vs WELFARE. Reaction-trained: asks 25%, complaints 14%, stranded 8%, welfare 0.64; welfare-trained: asks 48%, complaints 25%, stranded 0%, welfare 0.94. Fits in better, helps worse; silent harms escape feedback (RLHF analogy); bad local norms → groups & norms session.

Also: honest "What we simplified" (people's behavior fixed rather than solved jointly; one decision per encounter; truthful answers; known ask cost) and a **CIRL ↔ Marx/Mead mapping table** (counts = shared meaning from joint activity; prior = generalized other; likelihood = significant symbols; asking = language; Town B robot = socialized elsewhere / LLM; teaching = role-taking; reaction-trained = conformity).

## Exercises (runnable sweeps, no coding)
1. **Curiosity** for the Town B robot: 0 / 0.05 → stranded 18%; 0.2 → 4%; 0.5 → 2% (but complaints 25% → 45%). Who bears the cost of curiosity?
2. **A proud town**: proud share 0.7 → stops asking lookers (helps them blind, complaints up); 0.9 → stops asking "none" people, 8% stranded.
3. **How much teaching helps**: exaggeration 0 → 1 raises first-50 welfare 0.66 → 1.37, asks 44% → 29%.
4. **Making silent harms loud**: reaction-trained robot stops stranding once stranding is penalized more than ≈ −1.2 (−1 still strands 8%; −2 matches welfare-trained). Complaint channels, ombudspersons, audits.
5. **The robot with other things to do**: devotion (welfare vs own effort; help −3, ask −1 then help) 1.0/0.8 = pure CIRL; 0.6 stops asking "none"; 0.4 stops asking lookers; 0.2 ignores everyone. Mixed motives return.

## Verification
Executed end-to-end with nbconvert: 0 errors; all numbers above are from that run. Delivered without outputs.
