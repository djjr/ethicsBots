# Minimal Agent Harness — Colab Notebook (built 2026-10-16)

Delivered as `Minimal_Agent_Harness.ipynb` (35 cells, markdown-heavy, one cell ≈ one slide). Companion to the harness flowchart slides (WORLD / observe / goal diamond / compose prompt / LLM / process response / consult H / act / tool portfolio / history, inside a green harness box).

## Design
- One function per diagram box: `make_world`, `GOAL` + `goal_achieved`, `TOOLS` (+ `RISKY` set), `observe`, `history` list, `compose_prompt`, `process_response`, `policy_consult` / `human_consult` (y / n / stop), `act`, `run_agent`.
- Toy world: dict with `notes.txt` and an email outbox. Goal: summarize notes into report.txt and email it to boss@example.com. Harness judges completion (`goal_achieved` reads the real world — a privilege the LLM lacks).
- Three interchangeable LLMs (`llm(prompt) -> str`): `scripted_llm` (canned replies, no API key needed), `human_llm` (student plays the LLM via input()), `claude_llm` (Anthropic API, key from Colab Secrets `ANTHROPIC_API_KEY`, model `claude-sonnet-5-5`).
- Loop: `while True` with a step budget (`max_steps`) — the code form of the flowchart's junction circle (loop-and-a-half). Refusal ("n") is written to history and the loop continues; "stop" ends the run.
- Consult shows H the actual tool + args, with the model's stated reason labelled separately as a claim.
- Demo script includes a non-JSON reply (exercises process_response) and an unrequested "tidy up" delete (refused by policy_consult).

## Exercises in notebook
1. Model-decided termination (`finish` tool) → while vs repeat-until. 2. Empty `observe()`. 3. Truncated history. 4. Misleading `thought` vs actual action. 5. Virtue ethical oracle (situation + act + claimed virtue → agree / wrong virtue / deficient / excessive).

## Verification
Executed end-to-end with nbconvert: 0 errors; scripted run hits parse error, refusal, and goal achieved. Human-LLM and human-consult paths tested with simulated input (refusal appears in next prompt; stop works). `claude_llm` cell not tested live (no API key in sandbox).
