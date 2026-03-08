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


# Shot Meaning

Shot = One example given to the model before the final task.

0 examples → Zero-shot
1 example → One-shot
2+ examples → Few-shot

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
---

# Chain-of-Thought (CoT) Prompting

---

## Definition

Chain-of-Thought means you tell the model to **think step by step**
before giving the final answer.

Instead of jumping to the answer, the model shows its **full reasoning process**.

> **"Chain"** = a chain of reasoning steps, connected one after another.

---

## Simple Analogy

Imagine you ask a student: *"What is 15% of 280?"*

**Bad student:** Just writes `42` — you have no idea if they understood or guessed.

**Good student** writes:
- 10% of 280 = 28
- 5% of 280 = 14
- 10% + 5% = 15%
- 28 + 14 = **42**

Same answer. But now you can **see the thinking.**
If they made a mistake — you can see exactly **where it went wrong.**

That is Chain-of-Thought. **The model = the good student.**

---

## Example 1 — Without CoT ❌

**Prompt:**
```
A store has 3 shelves. Each shelf has 12 items.
They sold 10 items on Monday and 8 items on Tuesday.
How many items are left?
```
**Output:**
```
18
```
❌ Wrong. And you have no idea why.
No reasoning shown = no way to debug = **dangerous in production.**

---

## Example 2 — With CoT ✅

**Prompt:**
```
A store has 3 shelves. Each shelf has 12 items.
They sold 10 items on Monday and 8 items on Tuesday.
How many items are left?

Think step by step.
```
**Output:**
```
Step 1: Total items = 3 shelves × 12 items = 36 items
Step 2: Total sold = 10 (Monday) + 8 (Tuesday) = 18 items
Step 3: Items left = 36 - 18 = 18 items

Answer: 18
```
✅ Correct. AND you can see every step.
If it was wrong — you'd know **exactly which step failed.**

---

## Example 3 — Real LLM Engineer Use Case (Code Review Bot)

**Prompt:**
```
You are a senior Python engineer.
Review this function and decide if it has any bugs.
Think step by step before giving your verdict.

def calculate_average(numbers):
    return sum(numbers) / len(numbers)
```
**Output:**
```
Step 1: The function takes a list called 'numbers'.
Step 2: It calculates sum(numbers) — this works fine.
Step 3: It divides by len(numbers) — this will CRASH if the list is empty.
        ZeroDivisionError will be raised.
Step 4: There is no check for an empty list.

Verdict: Bug found. Add a check — if not numbers: return 0
```
✅ Without CoT the model might just say *"looks fine."*
With CoT it **caught the bug AND explained why.**

---

## Why This is Critical for Output Evaluation

When you evaluate LLM outputs at scale you need to know:
- Not just **what** the model answered
- But **why** it answered that way
- And **where** the reasoning broke down

CoT gives you that visibility.
```
No CoT   →  Black box          →  Hard to evaluate
With CoT →  Transparent reasoning  →  Easy to evaluate & debug
```
## The Most Important Use Case for YOUR Job
-As an LLM Engineer doing evaluation — CoT is not about getting the right answer.
-It's about seeing the reasoning so you can audit it.
-Without CoT → Model says "Approve the loan" 
              You don't know why. You can't audit it.

With CoT →   Step 1: Credit score = 750 ✅
             Step 2: Debt ratio = 0.3 ✅  
             Step 3: Employment = stable ✅
             Decision: Approve ✅
             Now you can audit every step.
That's why CoT matters for your role. Not for math. For transparency.
---

## Magic Phrases That Trigger CoT

| Phrase | When to Use |
|--------|-------------|
| `"Think step by step"` | General reasoning |
| `"Show your work"` | Math / calculations |
| `"Explain your reasoning before answering"` | Logic / decisions |
| `"Break this down step by step"` | Complex analysis |
| `"Walk me through your thinking"` | Debugging tasks |

---

## All 3 Types So Far — Full Comparison

| | Zero-Shot | Few-Shot | Chain-of-Thought |
|---|-----------|----------|------------------|
| **Examples given** | None | 2–5 | None needed |
| **Shows reasoning** | ❌ No | ❌ No | ✅ Yes |
| **Best for** | Simple tasks | Custom formats | Reasoning & logic |
| **Evaluation friendly** | ❌ Hard to debug | ⚠️ Medium | ✅ Easy to debug |
| **Magic phrase** | Just ask | Show examples | "Think step by step" |

---

## 📝 One Line for Your Notes

> **Chain-of-Thought = Tell the model to think step by step.**
> **It shows reasoning before the answer.**
> **Makes complex tasks accurate and makes debugging/evaluation easy.**
> CoT is not just for accuracy — it's for auditability. In evaluation, you need to see WHY the model decided, not just WHAT it decided.
