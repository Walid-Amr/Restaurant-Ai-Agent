<img width="1491" height="522" alt="Screenshot 2026-09-02 122320" src="https://github.com/user-attachments/assets/e610b033-2d25-4ae2-b704-15f3deb46fa6" />



# Restaurant-Ai-Agent
Building an AI-Powered Restaurant Ordering Assistant with n8n — A Case Study
The Problem

Small restaurants often rely on manual order-taking: a customer calls or messages, someone jots down the order, walks it to the kitchen, and later updates the inventory by hand. Every step is a chance for a mistake — a missed order, a stock count that's never updated, a customer who has to repeat their order because nobody wrote down their name or phone number correctly.

I wanted to see how far I could take this with pure automation: a customer messages a Telegram bot, the bot understands what they want in natural language, confirms the order, logs it, and keeps inventory in sync — with zero manual data entry.

The Solution

I built a multi-stage automation system in n8n, connected to Google Sheets and Telegram, with an AI agent at its core. The final piece — and the focus of this report — is the ordering assistant itself: a conversational AI that talks to customers, understands their order in plain language (in whatever language they write in), collects what's needed to complete it, and writes it straight into the restaurant's order log.

Technical Architecture

1. Telegram Trigger The flow starts the moment a customer sends a message to the restaurant's bot.

2. AI Agent This is the core of the system. It's given a system prompt containing the full menu (items, prices, ingredients) and a set of behavioral rules: reply in the customer's language, never assume a choice the customer hasn't confirmed, ask clarifying questions instead of guessing, and only mark an order as "chosen" once both a specific item and the customer's name and phone number have been provided.

The Agent is wired to three sub-components:

Model — an OpenRouter-connected chat model that powers the actual reasoning and replies.
Memory — a Window Buffer Memory node, keyed to each customer's Telegram chat ID, so the Agent remembers earlier messages in the same conversation (what they ordered, whether they've given their name yet, etc.).
Output Parser — a Structured Output Parser that forces every response into a strict JSON schema (message, is_chosen, name, price, ingredients, customer_name, customer_phone), with its own connected model to auto-fix malformed output when needed.

3. Send a text message The Agent's reply (message) is sent straight back to the customer on Telegram — whether that's a clarifying question, a suggestion, or an order confirmation.

4. Switch Branches the flow based on is_chosen. If the order isn't finalized yet (still gathering details), the flow stops here — the customer just gets the reply. If the order is confirmed, it moves on to be recorded.

5. Get row(s) in sheet2 → Edit Fields → Append row in sheet → Update row in sheet2 This is the order-logging and counter chain: the current order number is read from a counter sheet, incremented by one, used to log the new order (item, customer name, phone, date, status) into the main orders sheet, and the incremented counter is written back — so every order gets a unique, sequential number.

Challenges & Fixes

Building this wasn't a straight line. A few of the more interesting problems along the way:

Silent duplicate order numbers. Two different orders ended up with the same order number. Tracing it back, the "increment the counter" step was entirely missing from this branch — every order was reading the same stale number instead of incrementing it.
A hardcoded status bug. While fixing the above, I found every new order was being logged with status "Done" instead of "In kitchen" — meaning orders looked complete the instant they were placed, before the kitchen had even seen them.
Case-sensitive spreadsheet headers. Google Sheets column matching in n8n is case-sensitive; a header typed as "Last number" instead of "Last Number" silently broke every downstream reference to it.
The AI "forgetting" the conversation. Early on, the bot would re-greet a customer who had already chosen a sandwich, instead of continuing the order. The fix required moving from a stateless LLM chain to an AI Agent node with proper session-based memory, keyed to each customer's chat ID.
Malformed AI output. The model would occasionally wrap its JSON reply in markdown formatting or nest it under an unexpected key, breaking the structured parser. This needed both explicit formatting instructions in the prompt and a fallback auto-fix step.
Rate limits on free AI models. Heavy testing against a free-tier model hit its daily rate limit quickly, which meant pacing test runs and understanding exactly how usage, credits, and rate limits work on the model provider's platform.
Result

A customer can now message the bot in Arabic, English, or French, get a natural reply, confirm a sandwich, provide their name and number conversationally over a few messages, and have the order logged automatically with a sequential order number and correct kitchen status — no manual spreadsheet work at any point.

Lessons Learned

Most of the hardest bugs here weren't in the AI logic — they were in the plumbing around it: a missing connection, a hardcoded value left over from an earlier version, a column name that didn't match exactly. Debugging an AI-driven workflow means checking the deterministic parts (the sheet, the counters, the matching columns) just as carefully as the AI's behavior itself.

Built with n8n, Google Sheets, and Telegram, with Claude (Anthropic) used throughout as an AI pair-programmer — for debugging each stage, refining the prompt logic, and, toward the end, connecting directly into the n8n workflow via its MCP integration to inspect and fix the configuration directly.
