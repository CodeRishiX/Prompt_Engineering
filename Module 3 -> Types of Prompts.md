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
# Type 1 -- Zero-Shot Prompting


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



# Type - 2 Few-Shot Prompting

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

# type 3 -Chain-of-Thought (CoT) Prompting

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

---
# Type 4 — Role Prompting

## Definition
> Role Prompting means you tell the model **who to be** before giving it a task.
> You assign it an identity, expertise, and perspective — and it responds from that role.

**"Role"** = a character, expert, or persona the model plays throughout the conversation.

---

## The Simplest Way to Think About It

Imagine you have **one very smart friend** who can be anything.

You can say:
- *"Act as a doctor — explain my symptoms"*
- *"Act as a lawyer — review this contract"*
- *"Act as a Python expert — review my code"*

Same friend. Same brain. But completely different **tone, depth, vocabulary, and focus** based on the role you give.

That is Role Prompting.

---

## Example 1 — Without Role ❌

```
Prompt:
Explain what an API is.

Output:
An API (Application Programming Interface) is a way for 
two applications to communicate with each other...
(generic, surface level, no depth)
```

---

## Example 2 — With Role ✅

```
Prompt:
You are a senior backend engineer with 15 years of experience.
Explain what an API is to a junior developer joining your team today.

Output:
Think of an API like a waiter in a restaurant.
You (the frontend) sit at the table and tell the waiter (API) what you want.
The waiter goes to the kitchen (backend/database), gets your order, 
and brings it back to you.
You never go into the kitchen yourself. The API handles that boundary.

In technical terms...
(deep, practical, uses analogy, speaks to a junior dev specifically)
```

✅ Same question. Completely different quality.

---

## Example 3 — Real Production Use Case

This is how role prompting is used in **real LLM applications:**

```
Prompt:
You are a strict JSON validator and data quality engineer.
Your job is to:
- Check if the input is valid JSON
- Identify missing required fields
- Flag any data type mismatches
- Never fix the data yourself — only report issues

Required fields: name (string), age (integer), email (string)

Input:
{
  "name": "Raj",
  "age": "twenty-five",
  "phone": "9876543210"
}
```

```
Output:
Issues found:
1. "age" → Expected integer, got string ("twenty-five") ❌
2. "email" → Required field missing ❌
3. "phone" → Not a required field, but present (warning only)

Total errors: 2
```

✅ The role made the model behave like a **strict, focused tool** — not a general chatbot.

---

## Why Role Prompting is Powerful in Production

Without a role the model tries to be helpful to everyone.
With a role the model becomes **specialized and consistent.**

```
No Role → Model is a generalist
          Answers vary in tone, depth, format
          Hard to predict behavior

With Role → Model is a specialist
            Consistent tone, depth, format
            Predictable, auditable behavior ✅
```

---

## Role Prompting + Other Types Combined

In real systems you never use just one type.
You combine them:

```
You are a senior data analyst. (Role)

Classify these support tickets as P1, P2, P3. (Instruction)

P1 = System down, P2 = Feature broken, P3 = Minor (Context)

Example 1: "Site is down for all users" → P1 (Few-Shot)
Example 2: "PDF export broken" → P2 (Few-Shot)

Think step by step before classifying. (CoT)

Ticket: "Login button missing on mobile app."
```

This is how **real production prompts** are written.
All 4 types working together.

---

## One Line to Write in Your Notes

> **Role Prompting = Give the model an identity before the task. It changes tone, depth, focus and makes behavior consistent and predictable in production.**

---

## Quick Recap — 4 Types So Far

| Type | One Line Summary |
|---|---|
| Zero-Shot | No examples, just ask |
| Few-Shot | Show examples, model follows your pattern |
| Chain-of-Thought | Think step by step, shows reasoning |
| Role Prompting | Give model an identity, controls behavior |

---

# Type 5 — Instruction Prompting

---

## Definition

Instruction Prompting means you write **extremely clear, specific, and
unambiguous instructions** for exactly what you want the model to do.

Nothing is left for the model to **assume or guess.**

> **"Instruction"** = every detail of the task spelled out completely.

---

## Simple Analogy

Imagine giving directions to someone in an unknown city.

**Vague directions ❌**
```
"Go straight, then turn somewhere near the market."
```
They will get lost. 100%.

**Clear directions ✅**
```
"Go straight for 2 blocks. Turn left at the red traffic light.
 Walk 50 meters. The building is on your right."
```
They will reach exactly where you want.

> Instruction Prompting = giving the model **clear directions**, not vague ones.

---

## Example 1 — Vague Instruction ❌

**Prompt:**
```
Write something about climate change.
```
**Output:**
```
Climate change is one of the most pressing issues of our time...
(Could be an essay, could be a poem, could be 5 lines or 50 lines,
formal or casual — you have no idea what you'll get)
```
❌ Unpredictable. Unusable in production.

---

## Example 2 — Clear Instruction ✅

**Prompt:**
```
Write a 3 sentence summary about climate change.
- Use simple English
- Target audience is high school students
- Do not use technical jargon
- End with one actionable tip students can do today
```
**Output:**
```
Climate change means the Earth is getting warmer because of
gases released by cars, factories, and deforestation.
This warming is causing extreme weather events like floods,
wildfires, and rising sea levels around the world.
You can help by reducing electricity use, eating less meat,
and planting trees in your community.
```
✅ Predictable. Consistent. Production ready.

---

## The 5 Rules of Good Instruction Prompting

### Rule 1 — Be Specific About Length
```
❌ "Write a short summary"
✅ "Write a summary in exactly 3 sentences"
```

### Rule 2 — Be Specific About Audience
```
❌ "Explain machine learning"
✅ "Explain machine learning to a 15 year old with no coding background"
```

### Rule 3 — Tell It What NOT to Do
```
❌ "Explain the bug in my code"
✅ "Explain the bug in my code. Do not fix it. Do not rewrite it.
    Only explain what is wrong and why."
```

### Rule 4 — One Task at a Time
```
❌ "Summarize this, translate it, and make it formal"

✅ Step 1: "Summarize this in 3 sentences"
   Step 2: "Now translate the summary to Hindi"
   Step 3: "Now make the Hindi version formal"
```

### Rule 5 — Use Constraints
```
❌ "List some Python tips"
✅ "List exactly 5 Python tips for beginners.
    Each tip must be 1 sentence.
    Start each tip with an action verb."
```

---

## Real Production Example — Customer Support Bot

**Prompt:**
```
You are a customer support agent for an e-commerce company.

When a customer sends a complaint, do the following:
1. Acknowledge their issue in 1 sentence
2. Apologize in 1 sentence
3. Give a solution in 1-2 sentences
4. End with "Is there anything else I can help you with?"

Rules:
- Never use the word "unfortunately"
- Never make promises about specific dates unless given that information
- Always stay polite and professional
- Response must be under 80 words total

Customer message: "I ordered 5 days ago and my package still hasn't arrived."
```
**Output:**
```
I understand your package has not arrived after 5 days.
We sincerely apologize for this delay.
Please share your order ID and we will immediately investigate
the status and ensure it reaches you as soon as possible.
Is there anything else I can help you with?
```
✅ Every rule followed. Every time. At scale.

---

## Why This Matters for Evaluation

When you evaluate LLM outputs — **bad instructions = unfair evaluation.**
```
Vague instruction   →  Unpredictable output  →
You don't know if the model failed or the instruction failed.

Clear instruction   →  Predictable output    →
You know exactly what to measure and where it went wrong.
```

> **You cannot evaluate what you cannot define.**
> Instruction Prompting forces you to **define success clearly.**

---

## 📝 One Line for Your Notes

> **Instruction Prompting = Leave nothing for the model to guess.**
> **Specify length, audience, format, tone, and constraints explicitly.**
> **Vague instructions = unpredictable outputs = impossible to evaluate.**

---

## Recap — 5 Types So Far

| Type | One Line Summary |
|------|-----------------|
| **Zero-Shot** | No examples, just ask |
| **Few-Shot** | Show examples, model follows your pattern |
| **Chain-of-Thought** | Think step by step, shows reasoning |
| **Role Prompting** | Give model an identity, controls behavior |
| **Instruction Prompting** | Spell everything out, leave nothing to guess |
