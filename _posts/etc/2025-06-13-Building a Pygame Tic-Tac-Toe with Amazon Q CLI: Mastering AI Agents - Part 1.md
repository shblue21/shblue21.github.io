---
title: "Building a Pygame Tic-Tac-Toe with Amazon Q CLI: Mastering AI Agents — Part 1"
date: 2025-06-13T00:00:00
last_modified_at: 2025-06-13T00:00:00
categories:
  - etc
tags:
  - Amazon Q CLI
  - Natural Language Processing
  - Game Development
  - AI Agent
toc: true
lang: ko
published: true
description: "A step‑by‑step guide to building a natural‑language–driven game with Amazon Q CLI and fully harnessing AI agents."
keywords:
  - Amazon Q CLI
  - Natural Language Processing
  - Game Development
  - AI Agent
---


> This post chronicles how I completed a Python **Pygame** Tic‑Tac‑Toe game using the terminal‑based AI agent **Amazon Q CLI** with nothing more than concise prompts.

## What Is an AI Agent?

An **AI Agent** is an autonomous system designed to reach a user‑defined goal by running a **“Observe → Plan → Act”** loop powered by an LLM. It can call external tools, access the file system, hit APIs, and continually refine its strategy based on feedback.

If a regular **LLM chatbot** is a **knowledge engine** that stops at “question → text answer,” an **AI Agent** is an **execution engine** that turns that knowledge into real‑world actions that change your system.

Put differently, the LLM is the *brain*; the agent adds *hands and feet* (tool usage and iterative autonomy) so it can finish larger goals on its own.

> **LLM vs. AI Agent Example**
>
> * **LLM chatbot:** “What’s the weather in Seoul?” → “It’s sunny today with a high of 25 °C.”
> * **AI Agent:** “Build a simple web app that shows Seoul’s weather in real time.”
>
>   1. **Observe:** Check current directory and installed libraries.
>   2. **Plan:** Write `weather.py` → call OpenWeather API → set up a Flask server → create HTML template → craft run script.
>   3. **Act:** `pip install flask requests`, generate code files, launch the local server.
>   4. **Feedback:** Open the page, confirm data loads, tweak CSS for UI polish.
>
>   The user typed a single sentence, but the agent completed the entire **code‑build‑run** cycle and produced a working app.

---

### Observe‑Plan‑Act (OPA) Cycle

To move beyond *text answers* and actually edit code or deploy systems, an agent must decide **what to do next** on its own and adjust its behavior based on results. That mechanism is the **Observe‑Plan‑Act (OPA) cycle**.

#### Why OPA Matters

1. **No observation → blind execution** — it might touch the wrong file or revisit already‑fixed issues.
2. **No plan → inefficiency** — without an optimal route (e.g., UI → logic → tests) iterations multiply.
3. **No feedback → unknown quality** — a self‑correcting loop is essential for reliability.
4. **With OPA → multi‑step workflow automation** — even long pipelines like *“code change → build → test → deploy”* can finish from a single prompt.

#### Phase‑by‑Phase Flow

1. **Observe** — Gather the current state: prompt input, command output, code, logs.
2. **Plan** — Draft an action sequence to reach the goal (e.g., *“add AI opponent to Tic‑Tac‑Toe”*).
3. **Act** — Interact with external tools: shell commands, API calls, file edits.
4. **Feedback** — Re‑observe results, update the plan, and loop.

---

### AI Agent Line‑Up

* **Amazon Q CLI:** Terminal‑focused development agent.
* **GitHub Copilot Agent mode:** Automates PR creation and refactoring tasks.
* **OpenAI Codex:** Software‑engineering agent.

## Q CLI Installation & Usage

![Amazon Q CLI](../../img/250613_qcli_1.png){: .align-center}
**Amazon Q CLI** is a terminal‑based AI agent that automates development tasks like code generation, refactoring, testing, and deployment through conversational prompts. It understands hundreds of CLI tools such as `git`, `docker`, and `aws`, and even suggests execution plans.

Installation is straightforward with the official installer — see the [installation guide](https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/command-line-installing.html). For Windows you can run it inside WSL; refer to the [Windows guide](https://dev.to/aws/the-essential-guide-to-installing-amazon-q-developer-cli-on-windows-lmh).

Because it must run inside WSL on Windows, the experience there can be a bit cumbersome.

![Autocomplete inside the terminal](../../img/250613_qcli_2.png){: .align-center}

On macOS, enabling *Shell Integration* unlocks handy features like inline command autocompletion.
