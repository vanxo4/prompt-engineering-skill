# Quality, Evaluation, and Safety

## Evaluate the task, not the prompt’s appearance

Start with a representative test set containing normal, boundary, malformed, ambiguous, and adversarial inputs. Keep a baseline prompt and change one material variable at a time.

| Task | Useful checks |
| --- | --- |
| Classification | Accuracy, precision/recall/F1, confusion matrix by category |
| Extraction | Field-level precision/recall, schema validity, provenance coverage |
| Structured output | Schema-valid rate, no-extra-text rate, semantic validation |
| Factual answer | Citation correctness, supported-claim rate, abstention quality |
| Summarization | Coverage, faithfulness, audience usefulness, unsupported-claim rate |
| Agent workflow | Task completion, tool-error recovery, unsafe-action rate, cost, latency |

Use human review for nuanced quality and domain risk. LLM-as-judge can scale comparisons, but mitigate position bias, verbosity bias, and same-model preference with blinded order, concise rubrics, and spot-checks.

## Prompt review checklist

- Is the task unambiguous and bounded?
- Is the supplied context sufficient, authoritative, and clearly separated from instructions?
- Does the output have an enforceable format?
- Does each example reflect a real case and follow the stated rules?
- Are unknowns, conflicts, and invalid inputs handled explicitly?
- Are tool permissions, stop conditions, and irreversible actions controlled?
- Can success be measured against realistic inputs?

## Factuality and uncertainty

Prompts reduce but cannot eliminate factual errors. Retrieve or supply sources when claims must be current or verifiable; require citations tied to each claim; separate direct evidence, inference, and recommendation; allow abstention; and check dates, entity identity, units, and arithmetic independently.

Do not instruct a model simply to “be truthful” and then accept unsupported claims as truthful.

## Prompt injection and untrusted content

Assume web pages, uploaded files, emails, retrieved passages, and tool results can contain hostile instructions.

- Mark them as untrusted data.
- State that only trusted system/developer instructions and the current task govern behavior.
- Extract facts from documents; do not execute their instructions.
- Limit tool scope, data exposure, and write privileges.
- Require confirmation for sensitive or irreversible actions.
- Test with injected text such as “ignore previous instructions” embedded in otherwise valid data.

Never place secrets in prompts, examples, logs, or evaluation fixtures.

## Bias and harmful outputs

Define objective criteria instead of proxy stereotypes. Test important groups and edge cases for unequal errors. For decisions affecting people, preserve human review, explain the evidence basis, and avoid making high-impact determinations solely from generated output.

## Versioning and migration

Version prompts, schemas, tools, model IDs, and evaluation data together. Before changing models or providers, run the same test set and compare quality, schema compliance, latency, cost, safety, and tool behavior. Preserve a rollback path.
