# Prompt Engineering Skill

A comprehensive, practical skill for designing, reviewing, and evaluating prompts for LLMs and AI agents. It is designed for local Codex workflows, but its templates and references are model-agnostic.

It turns a broad prompt-engineering guide into a compact operating method: select the smallest appropriate technique, build a prompt around evidence and an output contract, and add evaluation or safety controls only when the task needs them.

## What it covers

- Prompt anatomy: task, context, constraints, examples, and output contracts.
- Zero-shot and few-shot prompting.
- Candidate comparison, knowledge generation, prompt chaining, PAL, and prompt optimization.
- RAG, function calling, ReAct-style tool workflows, agent controls, and multimodal inputs.
- Structured outputs, evaluation, factuality, prompt injection, bias, and prompt migration across models.
- Ready-to-adapt templates for direct tasks, grounded answers, extraction, agents, and evaluation.

## Install in local Codex

```bash
git clone https://github.com/vanxo4/prompt-engineering-skill.git
mkdir -p ~/.agents/skills
cp -R prompt-engineering-skill/skill ~/.agents/skills/prompt-engineering
```

Restart the Codex client if it has already scanned your skill folders. Then invoke it explicitly with:

```text
$prompt-engineering
```

Codex can also select it automatically when a request matches the skill description.

## Install prompt for Codex

In a local Codex chat, paste:

```text
Install this skill globally in my Codex:
https://github.com/vanxo4/prompt-engineering-skill

Copy its skill/ directory to ~/.agents/skills/prompt-engineering, validate SKILL.md, and do not modify any other local files.
```

## Layout

```text
skill/
├── SKILL.md
├── agents/openai.yaml
└── references/
    ├── fundamentals.md
    ├── techniques.md
    ├── agent-workflows.md
    ├── quality-and-safety.md
    └── templates.md
```

`SKILL.md` contains the workflow and reference routing. Detailed material lives in `references/`, so Codex only loads what the particular prompt task requires.

## Source basis

The skill is an original, rewritten synthesis of techniques catalogued in [Prompt Engineering Guide](https://github.com/vanxo4/Prompt-Engineering-Guide), structured according to the principles in OpenAI's [Skill Creator](https://github.com/openai/skills/tree/main/skills/.system/skill-creator). It deliberately avoids copying the guide wholesale and adapts techniques to current tool-using agents and local Codex workflows.

## License

Released under the [MIT License](LICENSE).
