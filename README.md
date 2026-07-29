# l1nk 🔗

**A LinkedIn growth strategist you text like a friend.**

> Showcase repo — what it is, how it's shaped, and what I learned. Source is private.

---

## The idea

Every LinkedIn tool wants you to open a dashboard. I don't open dashboards. I open iMessage.

l1nk is an agent living behind a phone number. You text it. It knows your profile, your recent posts, and what's landed before, and it talks to you like a strategist who's read your account — not a content generator.

## What it does

- **Two-way chat over iMessage.** No app, no login, no tab.
- **Spark File.** Text `post idea: …` any time and it's captured. Text `show my ideas` and they come back. The whole point is catching an idea in the 8 seconds you have it.
- **Reads your account.** Profile, recent posts, comments — so advice is grounded in what you actually posted, not generic best practice.
- **Stage and approve.** It drafts a post, DM, or comment reply, sends it to you as a message, and does nothing until you reply "yes" or 👍 it.

## The rule that shaped everything

**The agent never writes content in my voice.**

Not "tries not to" — *can't*. The write path only accepts text I supplied. It's a type constraint, not a prompt instruction, because prompt instructions are suggestions and this one isn't.

So the loop is dictate-and-send, not ghostwrite-and-approve. The agent is good at *what to post about*, *when*, and *who to reply to*. It's not allowed to be my voice. That's the only asset I have on that platform.

Everything else follows from it:

- **Nothing autonomous.** Every outbound action is approval-gated.
- **A hard daily action ceiling**, enforced in code. Not a setting. Not a prompt. A number in a file that the agent cannot talk its way past.

## Architecture, roughly

```
phone  ──iMessage──▶  gateway webhook  ──▶  agent loop (typed tools)
                                              │
                          ┌───────────────────┼───────────────────┐
                          ▼                   ▼                   ▼
                    spark file          LinkedIn API        approval queue
                   (your ideas)      (read + gated write)   (nothing ships
                                                            without a yes)
```

Every capability the agent has is a typed tool with a narrow signature. If a tool can't express "post something I didn't write," the agent can't do it. That's the whole safety model, and it's a lot more durable than a paragraph of instructions.

## What I learned

**Approval friction is the product, not a tax on it.** I expected the yes/no step to feel annoying. It's the reason I trust it enough to leave running.

**A tapback is a beautiful API.** 👍 to approve is the lowest-effort confirmation gesture that exists on a phone. It made the difference between using this daily and abandoning it.

**Agents that live in existing surfaces get used.** Same agent behind a web app would have died in week one. The distribution channel *is* the retention mechanic.

**Stub your integrations first.** The whole conversational loop was built and tuned against fake profile data before a single real API was connected. It meant the interesting problems got solved while the boring ones were still fake.

## Status

Working prototype. Runs against my own account. Not a product — a proof that the strategist-you-text shape works.

---

Built by [Milo](https://hey.milo.gg)
