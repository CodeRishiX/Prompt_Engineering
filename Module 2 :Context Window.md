# Context Window in AI & Prompt Engineering

---

## What is a Context Window?

A **Context Window** is the maximum amount of text (tokens) that an AI model 
can "see" and process at one time — including your input AND its output.

> Think of it like the AI's **short-term memory**.
> Whatever fits inside the window = AI can use it.
> Whatever falls outside = AI completely forgets it.

---

## Simple Definition

| Term           | Meaning                                              |
|----------------|------------------------------------------------------|
| **Context**    | All the text the AI currently has access to          |
| **Window**     | The limit (size) of how much it can hold at once     |
| **Token**      | A small chunk of text (~1 token = ~0.75 words)       |

---

## Real-World Analogy

Imagine you are reading a book — but you can only hold  
**10 pages at a time** in your memory.

- If the answer is on page 3 and you're on page 8 → ✅ You remember it  
- If the answer was on page 1 and you're now on page 12 → ❌ Forgotten

That "10 pages" = your **context window**.

---

## How is it Measured? (Tokens)

| Model         | Context Window Size |
|---------------|---------------------|
| GPT-3.5       | 4,096 tokens        |
| GPT-4         | 8,192 – 128,000 tokens |
| Claude 3      | 200,000 tokens      |
| Gemini 1.5    | 1,000,000 tokens    |

> 1,000 tokens ≈ 750 words ≈ ~1.5 pages of text

---

## What Goes INSIDE the Context Window?

Everything counts toward the limit:

1. Your **System Prompt** (instructions given to AI)
2. Your **User Message** (what you typed)
3. **Conversation History** (all previous messages)
4. **AI's Response** (the output it generates)
```
[ System Prompt + Chat History + Your Message + AI Response ]
        ↑ All of this must fit within the context window
```

---

## What Happens When Context Window is FULL?

When the limit is exceeded, the AI starts **forgetting** the oldest messages.
```
Turn 1:  User: "My name is Rahul."         ← enters context
Turn 2:  User: "I am learning Python."     ← enters context
Turn 3:  User: "I like chess."             ← enters context
...
Turn 20: User: "What is my name?"
AI:      "I don't know your name."         ← Turn 1 was pushed out!
```

---

## Use of Context Window in LLMs (Large Language Models)

| Use Case                  | How Context Window Helps                        |
|---------------------------|-------------------------------------------------|
| **Chatbots**              | Remembers the full conversation so far          |
| **Document Summarization**| Reads the entire document at once               |
| **Code Generation**       | Holds entire codebase for better suggestions    |
| **Q&A on long PDFs**      | Keeps the document in context while answering   |
| **Multi-turn dialogue**   | Maintains history for coherent responses        |

---

## Use of Context Window in Prompt Engineering

This is where context window becomes a **critical skill**.

### 1. Put Important Instructions EARLY
```
✅ Good:
"You are a Python tutor. Always explain with examples.
 Now explain list comprehension."

❌ Bad:
"Explain list comprehension.
 (500 lines of unrelated text...)
 By the way, always explain with examples."
```
> AI weighs early context more heavily. Don't bury your instructions.

---

### 2. Don't Waste Tokens on Irrelevant Text
```
❌ Wasteful Prompt:
"Hello! How are you? Hope you're doing well. 
 I just wanted to ask, if it's okay, could you maybe, 
 if possible, explain what a neural network is?"

✅ Efficient Prompt:
"Explain what a neural network is in simple terms 
 with one real-world example."
```
> Every wasted word = less space for useful context.

---

### 3. Use System Prompts Wisely
```
System Prompt (counts toward context window):
"You are a data science tutor for beginners.
 Always use simple language.
 Always give one real-world example.
 Never use jargon without explaining it."
```
> A well-written system prompt sets the AI's behavior  
> for the ENTIRE conversation within the window.

---

### 4. Summarize Long Conversations
When chat history gets long, summarize it to save tokens:
```
Instead of sending 50 messages of history, send:

"Summary so far: User is building a Flask API. 
 We have set up routes and are now working on authentication."

New Question: "How do I add JWT tokens?"
```
> This technique is called **Context Compression** — a key  
> prompt engineering strategy for long sessions.

---

## Key Takeaway

> The **Context Window** is the AI's working memory.  
> As a Prompt Engineer, your job is to:
> 
> ✅ Fit the most **relevant** information inside it  
> ✅ Place **critical instructions** at the beginning  
> ✅ Avoid **token waste** with vague or padded text  
> ✅ Use **summarization** when history grows too long  
>
> Master the context window = Master how AI thinks.
