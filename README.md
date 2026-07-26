# Prompt Skills

Prompts and reusable Skills personally crafted for different AI models.

The education section primarily focuses on TOEFL learning and responds in
Chinese. 教育部分的 Prompt 主要面向托福学习，并使用中文回复。

## Education

- [Learn a New Topic from Scratch](education/learn-a-new-topic.md) — Learn an unfamiliar subject step by step through adaptive questions and answers.

## Skills and Plugin

This repository is also a skills-only Plugin for ChatGPT and Codex. The original
Markdown prompts remain available for copying, while the Skills provide
automatically discoverable workflows.

### Included Skills

- `learn-new-topic` — Teach a complete beginner through adaptive,
  one-question-at-a-time tutoring.

### Install for local testing

Add this repository as a plugin marketplace:

```bash
codex plugin marketplace add jinyu-cai/prompt-skills --ref main
```

Then open the ChatGPT desktop app, select ChatGPT **Work** or **Codex**, open
**Plugins**, choose the marketplace source, and install **Prompt Skills**.

### ChatGPT web availability

Skills bundled in Plugins can run in ChatGPT **Work** on the web. A local
marketplace is used for desktop development and testing. To make this Plugin
available on the web, publish it to the universal Plugins Directory or share
the locally installed Plugin with an eligible ChatGPT workspace.

In a regular ChatGPT conversation without Work or Plugins access, copy and use
the Markdown prompt from the Education section instead.
