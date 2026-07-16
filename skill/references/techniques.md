# Prompting Techniques

## Selection summary

Use the least complex pattern that addresses the failure mode. A complex prompt is not inherently a robust prompt.

| Technique | Use when | Avoid when |
| --- | --- | --- |
| Zero-shot | Task and format are clear | Output boundaries or domain conventions are ambiguous |
| Few-shot | Examples teach the desired mapping | Examples are unrepresentative or expensive |
| Self-consistency / candidate generation | Independent attempts can reduce a reasoning error | The task has one verifiable computation or needs low latency |
| Generate knowledge | Background facts must be surfaced before a synthesis | Evidence must be current or source-grounded |
| Prompt chaining | Intermediate artifacts are useful and reviewable | One direct prompt is sufficient |
| Tree/graph exploration | Alternatives genuinely need comparison | A simple decision can be made directly |
| PAL / tool-assisted computation | Code or a calculator can verify the result | The computation is trivial and tool overhead dominates |
| Automatic prompt search | You have a dataset and measurable target | You only need one carefully written prompt |

## Direct and zero-shot prompting

State the task, constraints, input, and output contract. It works well for extraction, classification, rewriting, and common generation tasks.

```text
Classify the support message as billing, technical, account, or other.
Return only JSON matching {"category": "...", "confidence": 0.0}.
If the message does not provide enough information, use category "other" and confidence below 0.5.

Message:
{{message}}
```

## Few-shot prompting

Place a small set of representative input/output demonstrations before the live input. Use examples to show label boundaries, transformations, and exact formatting.

```text
Convert each title to a URL slug.

Input: “Prompt Engineering 101”
Output: "prompt-engineering-101"

Input: “R&D: A Practical Guide”
Output: "r-and-d-a-practical-guide"

Input: {{title}}
Output:
```

Test whether examples improve results. More demonstrations can degrade quality by consuming context or creating accidental rules.

## Decomposed reasoning, self-consistency, and verification

For difficult analysis, ask for an answer with observable intermediate deliverables rather than hidden reasoning. Examples: list assumptions, show formula substitutions, identify sources, or provide a verification checklist.

Use multiple independent candidate answers only when the result can be selected by a rule, a judge rubric, code, or human review. This is the useful core of self-consistency prompting. Avoid majority voting over shared misconceptions.

## Generate-knowledge prompting

Use a preliminary stage to collect relevant concepts, definitions, or hypotheses, then a second stage to apply them to the task. Keep generated background separate from verified evidence.

```text
Stage 1: List the concepts needed to assess {{problem}}. Mark each as known, assumed, or requiring evidence.
Stage 2: Solve the task using only verified evidence and explicitly labelled assumptions.
```

Do not use this technique to replace retrieval for current, regulated, or factual claims.

## Prompt chaining

Split work when each stage produces an artifact that can be checked before the next stage.

```text
1. Extract claims and source IDs.
2. Check each claim against the sources.
3. Draft the answer from verified claims only.
4. Validate required format and unresolved uncertainty.
```

Define the input and output contract of every stage. Preserve only useful state. Stop and request clarification if a required stage lacks evidence instead of allowing later stages to invent it.

## Candidate trees and graph prompting

For strategy, design, or planning, generate a small set of materially distinct options. Evaluate each against explicit criteria such as cost, risk, reversibility, time, and evidence. Select one or present a bounded trade-off.

For graph-shaped tasks, explicitly define the nodes, relationships, and permitted traversals before asking for inference. Use graph prompting when the answer depends on multi-hop relationships, not simply because the input can be drawn as a graph.

Do not require the model to expose private “thought” traces. Request a concise comparison table and recommendation rationale that a user can audit.

## Program-aided language models (PAL)

When a task has deterministic arithmetic, data transformation, parsing, or simulation, instruct the system to use code or a calculator and report the result with inputs and checks.

```text
Use a calculation tool for all arithmetic. State the formula, substituted values, result, and any unit conversion. Do not estimate when exact input is available.
```

Validate generated code in the real execution environment; a convincing code snippet is not evidence that it ran.

## Automatic, directional, active, and knowledge-guided prompting

If optimizing a production prompt, define a held-out evaluation set, a baseline, a score, and constraints such as cost and latency. Generate alternatives, evaluate them consistently, and retain the smallest improvement that generalizes. This is the practical form of automatic prompt engineering (APE).

- **Directional stimulus:** Supply concise domain cues or desired coverage when a task benefits from a steering signal; do not let cues become unsupported facts.
- **Active prompting:** Route uncertain or failed cases to human review or request additional labels/examples, then improve the prompt from informative failures.
- **Knowledge-guided prompting:** Retrieve or verify domain knowledge before generation; preserve provenance and uncertainty.
