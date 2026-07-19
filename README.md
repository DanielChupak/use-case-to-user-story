# use-case-to-user-story

A Cursor skill that turns one vague customer use case into a single, compelling, demo-ready Port user story. It's the first step of the **Build an Agentic Platform in 90 Minutes** workshop.

## What it does

Give it one messy sentence about a customer, and it produces a demo-ready user story — persona + goal + measurable value — positioned on Port's differentiation. It adds an **agentic extension**: an action turned into an autonomous, conditional workflow (e.g. a dev request auto-opens a PR, a prod request routes to approval).

It deliberately stops at the story and the acceptance **outcomes**. It does **not** design blueprints, properties, or a data model — that's the job of Cursor and the official `port-*` skills. Handing the builder a finished schema muzzles it; handing it strong outcomes sets it free.

## How it works — two gates

It's a conversation, not a one-shot. It stops and waits for you at two gates. Answer them honestly; if it invents the industry or the stack, the whole story is fiction.

1. **Intake** — you give the raw customer use case, one line, exactly as the customer said it.
2. **Discovery** — it asks three questions (industry, tech stack, and the "before" cost / current pain), then waits for your answers.

## Prerequisites

- [Cursor](https://cursor.com)
- A Port account (use **EU-prod** for the workshop)
- The official Port skills, which do the actual building: https://github.com/port-labs/port-skills

## Install

```bash
git clone https://github.com/DanielChupak/use-case-to-user-story
```

Then make the skill available to Cursor. <!-- TODO: replace with the exact step your setup uses, e.g. move/symlink the folder into your Cursor skills directory, or point Cursor at this repo. -->

## Use

In Cursor chat:

```
Use the use-case-to-user-story skill.

Customer: [customer name]
Raw use case: [one or two messy sentences — don't clean it up]

Run your full flow: ask me your intake and discovery
questions and wait for my answers. Don't assume anything.
```

Then answer the two gates.

## Output

- One user story — persona + goal + measurable value
- Acceptance criteria as **outcomes only** — no schema, no data model
- One autonomous agentic extension, labeled `[Natural fit]` or `[Stretch]`

## Part of the workshop

This is stage 1 of **Build an Agentic Platform in 90 Minutes**. From the story it produces, you then:

2. Spin up a fresh Port environment with the integrations you identified in discovery (for the out-of-the-box blueprints and relations)
3. Connect the Port MCP in Cursor, plan the build in Plan mode, then build
4. Clean the environment into a narratable demo flow
