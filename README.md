# omegarunner

*An LLM plays the Chrome T-Rex runner with MOSAIC multi-expert control.*

omegarunner replaces the keyboard in the Chrome T-Rex runner with MOSAIC (Wolpert-Kawato, 1998) multi-expert control, where each "expert" is a single LLM prompt that predicts what happens if it takes its own action, and the expert whose prediction matches reality earns trust and takes command — jump, run, or duck. No expert is ever trained: adding a new skill just means adding one more LLM prompt, never another neural network. The highlight is a side-by-side speed test on an identical course where both dinos judge correctly but the Cerebras runner (~50ms/decision) reaches the goal far sooner than a normal GPU (~1200ms/decision) — showing speed as throughput, not survival.

## Links

- **Google Colab:** https://colab.research.google.com/drive/1AjccApyq7zbJyuxozD1aeMcJtJ5SBhUo?usp=sharing
- **Demo video:** TODO
- **Live game:** [`trex.html`](trex.html)

## How it works

MOSAIC runs as a four-stage loop, once per obstacle:

1. **Prediction** — each expert forward-predicts `{safety, reward}` from a discrete view of the world.
2. **Verify** — at the result point, each prediction is compared against the real outcome.
3. **Responsibility** — the expert that predicted right earns trust (responsibility).
4. **Command** — a responsibility-weighted action is taken: jump / run / duck.

Decision/result gating keeps it honest: **one decision per obstacle, scored once**. And because **experts = actions**, adding a new action is just **one more prompt line** — the core loop is unchanged.

## Run it

- **Single play:** open `trex.html` in a browser; the MOSAIC AI drives automatically (mock by default).
- **Real LLM:** check **"use real Cerebras"**, enter an API key in the popup → a new game starts on real Cerebras.
- **Comparison mode:** toggle **"Comparison"** to see the slow-GPU vs Cerebras throughput race on one shared course.

## Credits

- **MOSAIC:** Wolpert & Kawato (1998), *"Multiple paired forward and inverse models for motor control."*
- Built with **Cerebras × Gemma**.
