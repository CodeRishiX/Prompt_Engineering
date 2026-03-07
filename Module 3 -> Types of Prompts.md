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
---



# Few-Shot Prompting

---

## Definition

Few-shot prompting is a technique where we provide the LLM with a **small number
of examples (shots)** before asking it to perform a task.

The model learns **YOUR pattern** from those examples and follows it —
not its own guess.

> **"Few" = 2 to 5 examples. Not too many, not zero.**

---

## Simple Analogy

Think of a **new employee**.

Before giving them work, you show them:
> *"Look, here are 3 solved examples. This is how WE do it here."*
> *"Now you do the next one the same way."*

The employee now follows **your rules** — not their own assumption.

That is Few-Shot.
**The examples = your training to the model.**

---

## When to Use Few-Shot Prompting

| Situation | Use Few-Shot? |
|-----------|---------------|
| You want a specific output format | ✅ Yes |
| You want consistent answers | ✅ Yes |
| Zero-shot gave vague results | ✅ Yes |
| Simple factual question | ❌ Not needed |

---

## Example 1 — Simple Pattern (Translation)

**Prompt:**
```
English → French

Hello → Bonjour
Thank you → Merci
Good night → 
```

The model learns the translation pattern from the examples.

**Output:**
```
Bonne nuit
```

---

## Example 2 — Custom Output Format (Customer Support Bot)

**Prompt:**
```
Reply to customer complaints using this exact format:
[Issue Identified]: ...
[Apology]: ...
[Solution]: ...

Example 1:
Customer: "My order never arrived."
[Issue Identified]: Missing delivery
[Apology]: We sincerely apologize for this inconvenience.
[Solution]: We will reship your order within 24 hours.

Example 2:
Customer: "I was charged twice for one order."
[Issue Identified]: Duplicate charge
[Apology]: We are sorry for the billing error.
[Solution]: A full refund will be processed within 3-5 business days.

Now reply to this:
Customer: "The product I received is damaged."
```

**Output:**
```
[Issue Identified]: Damaged product received
[Apology]: We sincerely apologize for sending a damaged item.
[Solution]: We will send a replacement immediately at no extra cost.
```

✅ Perfect format. Every single time. **Consistent. Predictable.**

---

## Why This Matters for Your Job as an LLM Engineer

In **production systems** you need consistent outputs.

> Not *sometimes* correct.
> Not *mostly* correct.
> **Always the same shape.**

Few-Shot is your tool to **enforce that consistency.**

---

## Few-Shot vs Zero-Shot

| | Zero-Shot | Few-Shot |
|---|-----------|----------|
| **Examples given** | None | 2 to 5 |
| **Model follows** | Its own training | Your pattern |
| **Output consistency** | Low | High |
| **Best for** | Simple tasks | Specific formats |
| **Weakness** | Vague output | Takes more tokens |

---

## 📝 One Line for Your Notes

> **Few-Shot = Give 2–5 examples of input → output before your question.**
> **Model learns YOUR pattern. Solves the biggest weakness of Zero-Shot.**
