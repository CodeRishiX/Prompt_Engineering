# Types of Prompting in Prompt Engineering

| # | Type | Core Idea |
|---|------|-----------|
| 1  | Zero-Shot | No examples, just instruction |
| 2 | One-Shot | One example given |
| 3 | Few-Shot | Multiple examples given |
| 4 | Chain-of-Thought | Think step by step |
| 5 | Self-Consistency | Multiple runs, majority answer |
| 6 | Role / Persona | Assign AI an identity |
| 7 | Instruction | Clear task + format + constraint |
| 8 | Contextual | Background info provided |
| 9 | ReAct | Reason then Act |
| 10 | Tree of Thought | Explore multiple paths |
| 11 | Maieutic | AI explains its own reasoning |
| 12 | Directive | Strict output control |
| 13 | Negative | Tell AI what NOT to do |
| 14 | Template | Fill-in-the-blank structure |
---

# Zero-Shot Prompting


## Definition

Zero-shot prompting is when we ask an LLM to perform a task
without giving any examples of how the task should be done.

The model relies only on its **pre-trained knowledge**
to understand the instruction and generate the response.

---

## Examples

**Example 1:**
```
Translate "Good morning" into Spanish.
```
No example translation was given — the model relies purely on its training.

**Example 2:**
```
Summarize this paragraph in 3 bullet points.
```
- No example
- Just instruction

That is **Zero-Shot**.

---

## Why It Works

Modern LLMs (GPT-4, Claude, etc.) were trained on **massive datasets**,
so they can often perform tasks without any examples.

---

## Limitation

Sometimes zero-shot is **not enough**.

> If we want a very specific output style,
> the model may struggle to get it right.

That's where the **next technique** comes in.
