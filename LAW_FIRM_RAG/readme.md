
# Motor Vehicle Law RAG Chatbot

An AI chatbot that answers legal questions about the **Provincial Motor Vehicles Ordinance 1965** by reading the actual law document and giving accurate, page-cited answers — instead of guessing from general knowledge.

---

## What This Project Does

You ask a question like:
> "What's the penalty for driving without a license?"

The chatbot:
1. Searches the actual law document for the relevant section
2. Pulls out the exact matching text
3. Uses an LLM to turn that text into a clear, simple answer
4. Tells you the page number where the answer came from

No hallucination, no guessing — every answer is grounded in the real document.

---

## How It Works (Simple Overview)

**Step 1: Break the document into chunks**
The full law document (110+ pages) is split into smaller pieces (chunks) so the AI can search through it efficiently.

**Step 2: Store the chunks in a vector database**
Each chunk is converted into a number-based representation (embedding) and stored, so the system can find the most relevant chunks for any question.

**Step 3: Retrieve relevant chunks**
When a question comes in, the system searches the vector database and pulls out the chunks most likely to contain the answer.

**Step 4: Generate the answer**
Those chunks are given to an LLM along with the question. The LLM is instructed to answer **only** using what's in the chunks — nothing made up.

**Step 5: Evaluate the answers**
Every answer is checked against two things:
- **Faithfulness** — Did the AI only use facts from the document?
- **Answer Correctness** — Does the answer match the real, correct answer?

This is how I found and fixed real problems in the pipeline (see below).

---

## Problems I Found and Fixed

### Problem: Broken chunks
Some legal sections got cut in half during chunking — for example, a list of rules `(a), (b), (c), (d)` was separated from its heading (like "Refusal of registration"). This caused the AI to answer with only part of the information, missing the main point.

**Fix:**
- Increased chunk overlap so headings stay connected to their content
- Improved the prompt to tell the AI to look across *all* retrieved chunks and combine fragmented lists instead of trusting only the "cleanest" looking chunk

**Result:** Answer accuracy went from 0.4 → 1.0 on affected questions, with no drop in faithfulness.

### Problem: Conflicting information in the source document
The law document has multiple versions of the same fine table (an older version and a newer amended version). The AI sometimes mixed them up.

**Fix:** Added a prompt instruction to always use the most recently amended version and clearly state which version was used.

---

## How I Tested It

I built a simple evaluation process (inspired by RAGAS):

1. Pick a real question from the document
2. Get the AI's answer and the retrieved context
3. Compare the answer against the actual ground truth text from the document
4. Score it on:
   - **Faithfulness** (0–1): only using real document facts
   - **Answer Correctness** (0–1): matching the real answer
5. If a score was low, trace it back to the cause — usually either a **retrieval problem** (wrong or broken chunk) or a **prompt problem** (right chunk, but bad explanation)
6. Fix the pipeline, then re-test the same question to confirm the fix worked

---

## Tech Used

- **Python** — core logic
- **LangChain** — RAG pipeline orchestration
- **Vector Database** (Qdrant) — storing and searching document chunks
- **LLM** — generating natural language answers from retrieved context
- **RAGAS-style evaluation** — faithfulness and correctness scoring

---

## What I Learned

- A RAG chatbot is only as good as its **chunking strategy** — most of the real bugs came from broken chunks, not the LLM itself
- Testing with real questions and checking answers against ground truth is the only way to actually catch these issues
- Good prompt instructions can act as a safety net, but they can't fully fix a bad retrieval pipeline — both need attention
