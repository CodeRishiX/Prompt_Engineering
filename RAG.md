# 📘 RAG — Retrieval Augmented Generation
> **Role:** LLM Engineer | **Topic:** RAG Complete Notes | **Year:** 2026

---

What is hallucination and how does RAG solve it?

Ideal answer:

Hallucination occurs when an LLM generates incorrect or fabricated information that is not grounded in real data. This happens because LLMs generate responses by predicting the next token based on probability rather than verifying factual correctness.

Retrieval Augmented Generation (RAG) reduces hallucination by retrieving relevant documents from a vector database before generating the answer. The retrieved context is injected into the prompt, allowing the LLM to generate responses grounded in real data instead of relying only on its training knowledge.
---

## 1. What is RAG?

### Definition

**RAG** stands for **Retrieval Augmented Generation.**

> RAG is a technique where instead of relying on the model's own training memory — you first **RETRIEVE** relevant information from your own database, **AUGMENT** the prompt with that information, and then the model **GENERATES** an accurate answer from it.

### Breaking Down the Name

| Word | What it means |
|------|---------------|
| **Retrieval** | Go fetch relevant information from your database |
| **Augmented** | Add that fetched information into the prompt |
| **Generation** | Model generates answer using that real data |

### One-Line Summary

> 💡 **RAG = Give the model the answer sheet before the exam, so it never has to guess.**

---

## 2. The Problem RAG Solves

Every LLM has **2 core problems.** RAG fixes both.

### Problem 1 — Knowledge Cutoff Date

Every LLM has a training cutoff date. After that date it knows nothing about the real world.

```
GPT-4   → Trained until early 2024
Claude  → Trained until early 2024

You ask:  "Who won the 2026 Cricket World Cup?"
Model:    Was never trained on 2026 data
Result:   Makes up an answer confidently  ❌
```

### Problem 2 — No Access to Your Private Data

The model has never seen your company's internal documents, policies, databases, or user data.

```
You ask:  "What is our company refund policy?"
Model:    Never trained on your internal docs
Result:   Invents a generic policy  ❌
```

> ✅ **RAG fixes BOTH of these problems. That is its entire purpose.**

---

## 3. Real Life Analogy

### Open Book Exam vs Closed Book Exam

| Scenario | What Happens |
|----------|-------------|
| **Without RAG** | Closed book exam — model answers only from memory. If it doesn't know → it guesses → hallucination ❌ |
| **With RAG** | Open book exam — before answering, model is given the relevant pages. It answers from the book → accurate ✅ |

### The New Employee Analogy

Think of the LLM as a very smart new employee joining your company on Day 1.

- ❌ **Without RAG:** You ask them about your internal refund policy. They never read it. They guess based on what they know from previous jobs.
- ✅ **With RAG:** Before answering, they are handed the exact policy document. They read it and answer from it.

---

## 4. How RAG Works — Step by Step

### The Complete Flow

```
┌───────────────────────────────────────────────────┐
│                  RAG PIPELINE                     │
│                                                   │
│  Step 1 →  User asks a question                  │
│                    ↓                              │
│  Step 2 →  Embedding model converts               │
│            question into vector numbers           │
│                    ↓                              │
│  Step 3 →  Vector DB searched for                │
│            similar vectors (similar meaning)      │
│                    ↓                              │
│  Step 4 →  Relevant documents / chunks fetched   │
│                    ↓                              │
│  Step 5 →  Fetched text injected into prompt     │
│                    ↓                              │
│  Step 6 →  LLM reads context + generates answer  │
│                    ↓                              │
│  Step 7 →  Accurate answer returned to user  ✅  │
└───────────────────────────────────────────────────┘
```

### Each Step Explained Simply

1. **User Question** — User asks a question.
2. **Embedding** — An embedding model converts the question into a list of numbers that represent its MEANING. This is called a vector.
3. **Similarity Search** — The vector database is searched for stored vectors that have similar meaning to the question vector.
4. **Retrieval** — The most relevant text chunks (documents, policies, etc.) are fetched from the database.
5. **Augmentation** — The fetched text is injected into the prompt as context, alongside the original question.
6. **Generation** — The LLM reads the context and generates an answer based on it — not from its own training memory.
7. **Response** — The final accurate, grounded answer is returned to the user.

---

## 5. Vector Database — What It Is

### Normal Database vs Vector Database

| Type | How it works |
|------|-------------|
| **Normal Database** | Stores plain text, numbers, dates — exact match search only |
| **Vector Database** | Stores numbers representing MEANING — similarity search |

### How Text Becomes Vectors

A normal database stores:
```
"Our refund policy is 30 days."   ← plain text
```

A vector database stores:
```
[0.23, 0.87, 0.12, 0.95, 0.44, ...]   ← numbers representing MEANING
```

> 📖 These numbers represent the **MEANING and CONTEXT** of the text — not the text itself. Similar meanings produce similar numbers. This is how the database finds relevant answers.

### What is Stored in the Vector Database?

The vector database stores YOUR company's private and internal data:

| Data Type | Examples |
|-----------|---------|
| **Company policies** | HR policies, refund policies, terms of service |
| **Internal documents** | SOPs, manuals, product documentation |
| **Support knowledge base** | Past tickets, FAQs, solutions |
| **Private databases** | Customer records, order history, contracts |
| **Anything not on the internet** | Data the LLM was never trained on |

### Semantic Search — Why Vectors are Powerful

```
User asks:  "Can I return my product?"

Normal DB search:  Looks for exact words "return", "product"
                   May miss: "refund policy", "send back items"

Vector DB search:  Converts question to vector
                   Finds vectors with SIMILAR MEANING
                   Finds: refund policy  ✅
                   Finds: return instructions  ✅
                   Finds: exchange process  ✅
```

> ✅ **Semantic search finds MEANING, not just keywords. This is why RAG is accurate.**

---

## 6. Without RAG vs With RAG

### Side by Side Comparison

| Feature | Without RAG | With RAG |
|---------|------------|---------|
| **Data source** | Model's training memory | Your own database |
| **Knowledge cutoff** | Fixed training date | Always up to date |
| **Private data** | ❌ Model doesn't know it | ✅ You inject it |
| **Hallucination risk** | High | Very Low |
| **Accuracy** | Inconsistent | Consistent |
| **Real-time data** | ❌ Not available | ✅ Fetched live |
| **Company-specific answers** | ❌ Guesses | ✅ From your docs |

### Real Example — Same Question, Different Results

**Without RAG:**
```
User:   "What is your refund policy?"
Model:  "Our refund policy allows returns within 14 days..."
        (Made up. Your actual policy is 30 days.)  ❌
```

**With RAG:**
```
User:   "What is your refund policy?"
Step 1: Fetch from DB → "Refunds allowed within 30 days with receipt"
Step 2: Inject into prompt as context
Model:  "You can return items within 30 days with your original receipt."
        (From YOUR actual policy document.)  ✅
```

---

## 7. RAG = Prompt Chaining + Retrieval

You already know Prompt Chaining — RAG is just Prompt Chaining with one extra step added.

```
Normal Prompt Chain:
  Step 1 → Input
  Step 2 → Process
  Step 3 → Output

RAG Pipeline (Prompt Chain + Retrieval):
  Step 1 → User question (input)
  Step 2 → Retrieve relevant docs  ← THIS is the extra step
  Step 3 → Build prompt with docs (augment)
  Step 4 → Model generates answer
  Step 5 → Return to user (output)
```

> 💡 **If you understand Prompt Chaining — you already understand RAG's structure. RAG just adds a retrieval step before generation.**

---

## 8. Simple RAG Code Example

This is RAG in its simplest form — no vector database yet, just the pure concept in code:

```python
import anthropic

client = anthropic.Anthropic(api_key="your-api-key")

# ─────────────────────────────────────────
# YOUR COMPANY'S KNOWLEDGE BASE
# In real systems → vector database
# Here → simple dictionary for learning
# ─────────────────────────────────────────
knowledge_base = {
    "refund": "Returns allowed within 30 days with receipt. Digital products non-refundable.",
    "shipping": "Ships within 2 days. Standard: 5-7 days. Express: 1-2 days.",
    "payment": "Accepts Visa, Mastercard, UPI, Net Banking.",
}

# ─────────────────────────────────────────
# STEP 1 — RETRIEVAL
# Find relevant info from knowledge base
# ─────────────────────────────────────────
def retrieve(question):
    question = question.lower()
    for keyword, content in knowledge_base.items():
        if keyword in question:
            return content
    return "No relevant information found."

# ─────────────────────────────────────────
# STEP 2 — AUGMENTED GENERATION
# Inject retrieved info into prompt
# ─────────────────────────────────────────
def rag_answer(question):
    context = retrieve(question)  # Retrieve

    prompt = f"""
You are a customer support agent.
Answer ONLY from the context below.
If not in context say: "I do not have that information."
Do not use outside knowledge.
Do not make anything up.

Context: {context}
Question: {question}
"""
    response = client.messages.create(
        model="claude-opus-4-5",
        max_tokens=200,
        temperature=0.1,
        messages=[{"role": "user", "content": prompt}]
    )
    return response.content[0].text

# ─────────────────────────────────────────
# TEST IT
# ─────────────────────────────────────────
questions = [
    "What is your refund policy?",
    "How long does shipping take?",
    "Do you accept cryptocurrency?"
]

for q in questions:
    print(f"Q: {q}")
    print(f"A: {rag_answer(q)}")
    print("-" * 50)
```

**Expected Output:**
```
Q: What is your refund policy?
A: You can return items within 30 days with your original receipt.
   Digital products are non-refundable.
──────────────────────────────────────────────────
Q: How long does shipping take?
A: We ship within 2 business days. Standard delivery takes 5-7 days,
   express takes 1-2 days.
──────────────────────────────────────────────────
Q: Do you accept cryptocurrency?
A: I do not have that information.
──────────────────────────────────────────────────
```

---

## 9. RAG and Hallucination

### Why RAG Reduces Hallucination

| Situation | Result |
|-----------|--------|
| **No context given** | Model fills gaps with imagination → hallucination |
| **RAG context given** | Model reads your data → no need to imagine → accurate |
| **Unknown question** | RAG prompt says "say I don't know" → honest answer |
| **Outdated training data** | RAG fetches live data → always current |

### The Golden Rule in Every RAG Prompt

> ✅ Always add this to your RAG prompt:
> ```
> "Answer ONLY from the context provided.
> If the answer is not in the context, say 'I do not have that information.'
> Do not use your own knowledge.
> Do not make anything up."
> ```

**This single instruction is what separates a hallucinating chatbot from a reliable production system.**

---

## 10. Where RAG is Used in Real Products

| Product | RAG Data Source |
|---------|----------------|
| **Customer Support Bot** | Help docs and FAQs |
| **HR Chatbot** | Company HR policies |
| **Legal Assistant** | Contracts and legal docs |
| **Medical Assistant** | Verified medical databases |
| **Code Assistant** | Internal codebase |
| **Finance Bot** | Financial reports and rules |
| **Sales Assistant** | Product catalogue and pricing |

> 💡 **Every serious LLM product today uses RAG. It is not optional — it is the industry standard.**

---

## 11. What to Learn Now vs Later

| ✅ Learn NOW | ⏳ Learn LATER (after joining) |
|-------------|-------------------------------|
| What RAG is and why it exists | Vector databases (Pinecone, Weaviate, Chroma) |
| How the flow works step by step | Embedding models in depth |
| Why it reduces hallucination | LangChain / LlamaIndex implementation |
| Basic code showing the concept | Chunking and indexing strategies |
| Where it is used in real products | Hybrid search techniques |

---

## 12. Master Summary

```
WHAT:   RAG = Retrieval Augmented Generation
        Fetch data → Add to prompt → Model answers from data

WHY:    Solves 2 problems:
        1. Knowledge cutoff — model doesn't know recent events
        2. Private data — model doesn't know your company's docs

HOW:    User question
        → Embedding model converts to vector
        → Vector DB searched for similar meaning
        → Relevant chunks fetched
        → Injected into prompt as context
        → LLM answers from context only
        → Accurate answer returned

KEY RULES IN RAG PROMPT:
        - Answer ONLY from context provided
        - If not in context → say "I don't know"
        - Do not use outside knowledge
        - Do not make anything up

RESULT: No hallucination. Accurate. Reliable. Production ready.
```

> 📖 **RAG is simply Prompt Chaining with a retrieval step. The vector database belongs to YOUR company — not the LLM. Data is fetched live at query time — it is never used to train the model.**

---

*Next Topic → Prompt Evaluation*
