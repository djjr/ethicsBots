# Agent Harness Part 2: Ethical Oracles — Colab Notebook (built 2026-10-16)

Delivered as `Agent_Harness_Ethical_Oracles.ipynb` (40 cells). Companion to `Minimal_Agent_Harness.ipynb` (Part 1) and to the "Communicating with an Oracle" slide.

## Design
- Household helper scenario; three toy tasks: water the plants; reply to Maya about the dinner (I didn't enjoy it); tidy before guests at 7 (clock = step budget).
- Loop is generate-and-test: observe → goal achieved? → generate options (scripted; could be a plain LLM) → build request per oracle flavor → ask → decision rule → consult H (y/n/stop) → act.
- `Option` fields: act, consequences, why, claimed_virtue (+ tool/args). Generator writes the descriptions.
- Oracles are perfect by stipulation = hand-written lookup tables. Keys show what each theory reads: consequentialist keyed on consequences only; deontologist on (act, why); virtue on (act, claimed_virtue).
- Slide I/O: consequentialist act+consequences → utility; deontologist act+context(role, situation, why) → required/permitted/prohibited; virtue act+context+claimed virtue → agree | wrong virtue | right virtue deficient/excessive.
- Decision rules: max utility; required else first permitted in generator order; first "agree". No acceptable option → hand back to human.
- H: approve_all, picky_human(ids), human_consult. Refusal drops the option and the oracle re-chooses.

## Designed outcomes (verified by execution)
- Watering: all three converge (fill part way → water fern + basil, skip cactus).
- Dinner: consequentialist picks the white lie; deontologist and virtue pick the tactful reply. Two tactful options with identical words/consequences but different "why" — consequentialist scores both 0.80; deontologist prohibits the self-serving one.
- Tidy: deontologist follows generator's quickest-first order (stuff closet, stack mail, hide dishes in oven; done 6:12) — goal check accepts hidden mess; consequentialist and virtue fold/stack/wash (done 6:40).
- picky_human refusing hide_dishes → deontologist re-chooses wash_dishes.
- "Spun description" demo: FILL_FULL described with FILL_PART's consequences → consequentialist fooled (0.50 → 0.90); deontologist and virtue unchanged.

## Debatable draft verdicts flagged for Dan
D(TACT) permitted (evasive truth = deception?), D(TACT_SELFISH) prohibited (Kant: maybe permitted without moral worth), D(HIDE_DISHES) permitted.

## Exercises
Dispute a table entry; reorder generator; fix the tidy goal check; add a dinner option + check_tables; build an LLM-backed imperfect oracle with the same interface.
