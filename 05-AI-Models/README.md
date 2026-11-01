# AI Models

All models run locally on the Mac Studio with Apple's MLX. **No cloud AI.**

| Model | Type | Used for |
| --- | --- | --- |
| Qwen3.6-27B | LLM, 4-bit | Evidence chat with citations, case summaries, structured second opinion |
| Qwen3.6-35B-A3B | MoE LLM, 4-bit (the light model) | Fast turns: Learning Ask, interviewer, key points |
| Qwen3-Embedding-8B | Embeddings | Finds similar past cases, ranked in PostgreSQL |
| Whisper large-v3-turbo | Speech → text | My spoken answers in the interview trainer |
| Kokoro-82M | Text → speech | Reads questions and answers aloud |
| wav2vec2 (phonemes) | Speech scoring | Pronunciation feedback |

## Cite or reject

* Atlas gives the model only the evidence it selected for this question
* The answer is strict JSON; **every sentence must cite** a field, a card fact or a computation Atlas supplied
* An answer that cites anything it was not given is **rejected**, not shown
* Atlas does the counting (frequency, rarity, lineage); the model only explains

## Log text is data, not instructions

A log line can contain attacker-written text: a User-Agent, a file name, a command line. Atlas passes it to the model as labelled data, never as an instruction, and neutralises model control tokens hidden inside it. An attempt is flagged on the answer card.

## One request at a time

* Investigations go first; learning waits
* A request is admitted only if memory and the context budget allow it; otherwise Atlas says **BUSY** with the reason
* If the model is unavailable, every saved fact is still there and the case can still be closed

## The model never decides

It has no route to decisions, playbook steps or closures. AI output is labelled "AI-generated · verify".
