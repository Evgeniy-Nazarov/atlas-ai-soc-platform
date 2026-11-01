# Memory — Atlas grows with the analyst

Atlas gets more useful from my decisions — **never by retraining a model on its own.** Every decision I record is kept as history and comes back as context.

## Prior decisions on the next alert

* The triage card shows what I decided on alerts like this one before
* The evidence chat can cite those decisions: *"you closed similar alerts as expected activity, here is why"*

## Similar cases

* Closed cases are indexed with a local embedding model (Qwen3-Embedding-8B)
* Atlas finds past cases by meaning, not only by rule number, and ranks them in PostgreSQL

## Noise rules — only with my approval

* Atlas suggests a noise rule when I closed the same rule on the same host as false positive or expected activity **at least 5 times, on at least 2 different days**, with no other outcome
* High-severity alerts (level 12+) are never covered
* A rule becomes active only after I accept it; it has a review date and expires on its own
* An active rule only **marks** matching alerts as "likely noise" — it never hides or closes them

## A golden set of my own cases

A small set of real lab alerts with my verdicts and the facts that decided them. It checks that the triage card shows those decisive facts and that the suggested playbook matches mine. Raw values stay on the Mac; only hashes are kept in Git.

## The loop

```text
I decide  →  Atlas remembers  →  the next alert comes with my history  →  I decide faster and better
```

The other half of the loop is the [Learning Centre](../07-Learning-Centre/): there the analyst grows with Atlas.
