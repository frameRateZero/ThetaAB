# θ WAVE — Attentional Blink Theta Estimator

A browser-based psychophysics tool that estimates your brain's theta oscillation frequency (~3–7 Hz) using the attentional blink paradigm.

## How it works

A rapid stream of gray letters flashes on screen. A salient **red O** resets theta phase via the LC-NE system. Your task: after each trial, say **Yes or No** — did you see the **X** that followed?

The lag between O and X (SOA, 50–600ms) is selected adaptively by **QUEST+** using a phase-reset damped sine likelihood model:

```
P(detect | SOA, f, A, base) = base − A · sin(2πf·SOA/1000) · exp(−SOA/250)
```

This encodes all three attentional blink landmarks:
- **Lag-1 sparing** — high detection just after T1 (~T/4)
- **Trough** — suppression at ~T/2 (the blink itself)  
- **Recovery** — return to baseline at ~T

Theta frequency `f` (3–7 Hz) falls directly out of the Bayesian posterior. 120 trials, ~6 minutes.

## Model details

| Parameter | Value |
|-----------|-------|
| Likelihood | Damped sine, τ=250ms |
| Frequency grid | 3.0–7.0 Hz, 20 steps |
| Amplitude grid | 0.30–0.75, 6 steps |
| Baseline grid | 0.88, 0.93, 0.97 |
| Combinations | 360 |
| SOA range | 50–600ms |
| Sampling | 70% QUEST+ / 30% uniform |
| Catch trials | ~25% (no T2, measures false alarm rate) |
| Response | Forced-choice Yes/No after each stream |

## Files

```
index.html    — entire app (single file, no dependencies)
manifest.json — PWA manifest
sw.js         — service worker (offline support)
icon-192.png  — PWA icon
icon-512.png  — PWA icon
```

## Running locally

Just open `index.html` in a browser. For full PWA features (offline, install prompt) serve over HTTPS or use:

```bash
npx serve .
```

## CSV Export

Each session exports a CSV with:
- Per-trial: trial number, type (probe/catch), SOA_ms, detected (1/0)
- Summary: posterior mean/MAP frequency, trough time, amplitude, confidence

## References

- Bonnefond & Jensen (2012). Alpha oscillations serve to protect working memory maintenance against anticipated distractors. *Current Biology*.
- Watson & Pelli (1983). QUEST: A Bayesian adaptive psychometric method. *Perception & Psychophysics*.
- Dux & Marois (2009). The attentional blink: A review of data and theory. *Attention, Perception, & Psychophysics*.
