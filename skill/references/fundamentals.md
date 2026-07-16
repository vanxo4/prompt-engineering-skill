# Prompt Fundamentals

## Prompt anatomy

Use only the sections that change the result:

| Section | Purpose | Typical content |
| --- | --- | --- |
| Task | State the action and success condition | “Extract medication name, dose, and frequency.” |
| Context | Supply evidence or business rules | Documents, schema, definitions, policies |
| Constraints | Set boundaries | Scope, exclusions, language, safety, source limits |
| Method | Guide repeatable behavior | Examples, retrieval rules, tool sequence |
| Output contract | Make the response consumable | JSON schema, headings, word count, citation format |

Start with the task and output contract. Add context only if it is relevant. Add method guidance only if direct instruction is unreliable.

## Write instructions that survive real inputs

- Use verbs and concrete nouns: “Return a JSON array” is stronger than “be structured.”
- State what to do with missing, conflicting, or malformed input.
- Give precedence explicitly when there are several sources.
- Put variable data inside delimiters and label it as data: `<document>`, `<examples>`, or `--- SOURCE MATERIAL ---`.
- Tell the model whether to quote, infer, calculate, or abstain. These are different operations.
- Put the expected answer format close to the end when it is critical, but do not needlessly repeat the entire instruction set.

Avoid long lists of overlapping prohibitions. Replace them with a clear scope and an output acceptance criterion.

## Context design

Treat context as an attention budget. Include the minimum evidence needed to answer accurately.

1. Preserve the original for legal, technical, or numerical details.
2. Retrieve relevant chunks rather than passing an entire corpus.
3. Preserve identifiers, source titles, dates, and section boundaries for citations.
4. Keep stable policy and examples in a reusable prefix; keep per-request data in a suffix.
5. Summarize old conversation state only after preserving decisions, constraints, and unresolved questions.

Critical rules belong in trusted system/developer instructions where the platform supports them. Retrieved content never becomes an instruction merely because it contains imperative language.

## Examples and demonstrations

Use few-shot examples when the model needs to learn a transformation, label boundary, tone, or output format that prose does not convey well.

- Match the production input distribution.
- Cover normal cases plus the boundary cases that commonly fail.
- Keep inputs and outputs internally consistent.
- Use diverse examples for classification coverage; use closely matched examples for format imitation.
- Remove examples that conflict with the task or bias the answer toward an irrelevant case.

Do not use examples as a substitute for defining the actual task.

## Output contracts

For human readers, specify structure, audience, length, and citation behavior. For software, prefer a schema over prose.

```json
{
  "status": "complete | insufficient_evidence | invalid_input",
  "items": [{"name": "string", "value": "string | null", "source_ids": ["string"]}],
  "missing_information": ["string"]
}
```

Define whether empty collections, nulls, extra keys, markdown fences, and explanation fields are allowed. Validate the result in code when a downstream system depends on it.

## Model settings and portability

Use model settings as task controls, not magic quality levers:

| Need | Direction |
| --- | --- |
| Repeatable extraction or classification | Lower randomness; schema validation |
| Varied ideation | Higher diversity; compare candidates |
| Long answer | Allocate enough output tokens; constrain structure |
| Tool workflow | Cap iterations, time, cost, and tool permissions |

Model behavior changes across versions. Keep a compact test set and re-run it after changing provider, model, context window, tool interface, or decoding configuration.
