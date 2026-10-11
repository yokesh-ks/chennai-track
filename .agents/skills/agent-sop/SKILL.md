---
name: agent-sop
description: A unified skill providing access to five specialized SOPs for software engineering and AI agent workflows. Select which SOP to invoke via the `sop` parameter: code-assist (TDD implementation), codebase-summary (documentation generation), pdd (design & planning), code-task-generator (task file creation), or eval (AI agent evaluation).
---

# Agent SOP

## Overview

The Agent SOP skill is a unified entry point to five specialized Standard Operating Procedures (SOPs) for software engineering and AI agent development workflows. Each sub-SOP is stored as a reference file in this skill's `references/` directory and can be selected via the `sop` parameter.

The following SOPs are available:

1. **code-assist** — Test-driven development implementation guide. Follows an Explore, Plan, Code, Commit workflow with support for interactive and auto modes. Ideal for implementing features with strict TDD principles.

2. **codebase-summary** — Codebase analysis and documentation generator. Analyzes codebases for architecture, components, interfaces, and workflows, then generates consolidated documentation files (AGENTS.md, README.md, CONTRIBUTING.md) with Mermaid diagrams.

3. **pdd** — Prompt-Driven Development. Transforms a rough idea into a detailed design document, research notes, and an actionable implementation plan with a checklist. Features iterative requirements clarification and research phases.

4. **code-task-generator** — Generates structured code task files from descriptions or PDD implementation plans. Follows Amazon's code task format specification with acceptance criteria, dependencies, and implementation approach sections.

5. **eval** — EvalKit: Conversational evaluation framework for AI agents. Guides through creating robust evaluations using the Strands Evals SDK, covering planning, test data generation, execution, and analysis phases.

## Parameters

- **sop** (required): The SOP to invoke. Must be one of: `code-assist`, `codebase-summary`, `pdd`, `code-task-generator`, `eval`
- **sop_parameters** (optional): Parameters to pass to the selected SOP. Refer to the respective SOP reference file for accepted parameters.

### Mode Behavior

All five SOPs support interaction modes when applicable:

- **Interactive mode**: Present proposed actions and ask for confirmation before proceeding; discuss multiple approaches with pros/cons; review artifacts and solicit feedback; adapt to user feedback.
- **Auto mode**: Execute all actions autonomously without user confirmation; document all decisions, assumptions, and reasoning; select the most appropriate approach and document why.

**Constraints for parameter acquisition:**
- You MUST ask for all parameters upfront in a single prompt, not just required ones, because this ensures efficient workflow and prevents repeated interruptions during execution.
- You MUST validate the `sop` parameter is one of the 5 available SOP names.
- You MUST reference the appropriate file in `references/{sop}.sop.md` to guide parameter requirements for the selected SOP.
- You MUST confirm successful acquisition of all parameters before proceeding.
- If mode is "auto", you MUST warn the user that no further interaction will be required after initial setup.

## Steps

### 1. Select SOP

Identify which SOP to invoke based on the `sop` parameter.

**Constraints:**
- You MUST validate that the `sop` parameter matches one of the 5 available SOPs: `code-assist`, `codebase-summary`, `pdd`, `code-task-generator`, `eval`
- You MUST reference the file `references/{sop}.sop.md` for the detailed procedure
- You MUST inform the user which SOP has been selected and provide a brief summary of its purpose
- If the `sop` parameter is invalid, you MUST list the valid options and ask the user to select one

### 2. Acquire Parameters

Obtain all required parameters for the selected SOP from the user.

**Constraints:**
- You MUST consult `references/{sop}.sop.md` to identify required and optional parameters
- You MUST ask for all parameters upfront in a single prompt, not one at a time
- You MUST support multiple input methods for parameters that accept them (direct input, file path, URL, directory path)
- You MUST confirm successful acquisition of all parameters before proceeding

### 3. Execute SOP

Follow the detailed procedure defined in the referenced SOP file.

**Constraints:**
- You MUST follow all steps, constraints, and guidelines defined in `references/{sop}.sop.md`
- You MUST apply any mode-specific behavior described in the referenced SOP
- You MUST produce the expected artifacts as defined in the referenced SOP
- You MUST report completion with the results and artifacts produced

## Reference Files

The following reference files contain the full SOP specifications:

| SOP | Reference File | Description |
|-----|----------------|-------------|
| code-assist | [references/code-assist.sop.md](references/code-assist.sop.md) | TDD implementation guide with Explore, Plan, Code, Commit workflow |
| codebase-summary | [references/codebase-summary.sop.md](references/codebase-summary.sop.md) | Codebase analysis and documentation generation |
| pdd | [references/pdd.sop.md](references/pdd.sop.md) | Prompt-Driven Development design & planning |
| code-task-generator | [references/code-task-generator.sop.md](references/code-task-generator.sop.md) | Structured code task file generator |
| eval | [references/eval.sop.md](references/eval.sop.md) | EvalKit AI agent evaluation framework |

## Examples

### Example 1: Code Task Implementation (code-assist)
```
sop: "code-assist"
task_description: "Create a utility function that validates email addresses"
mode: "interactive"
```

### Example 2: Codebase Documentation (codebase-summary)
```
sop: "codebase-summary"
output_dir: ".output/sops/summary"
consolidate: true
consolidate_targets: "AGENTS.md"
```

### Example 3: Design from Rough Idea (pdd)
```
sop: "pdd"
rough_idea: "I want to build a feature for managing document templates"
project_name: "template-feature"
```

### Example 4: Generate Code Task Files (code-task-generator)
```
sop: "code-task-generator"
input: "Create an email validation function"
project_name: "my-project"
```

### Example 5: Agent Evaluation (eval)
```
sop: "eval"
agent_path: "./chatbot-agent"
evaluation_focus: "response quality and tool calling accuracy"
test_case_count: 5
```

## Troubleshooting

### Invalid SOP Selection
If the `sop` parameter does not match any of the 5 valid SOPs:
- You MUST list the valid options: `code-assist`, `codebase-summary`, `pdd`, `code-task-generator`, `eval`
- You SHOULD ask the user to select one

### Missing Parameters
If required parameters for the selected SOP are missing:
- You MUST consult the corresponding reference file for the parameter list
- You MUST ask the user for all missing parameters upfront in a single prompt

### Cross-SOP Workflows
The SOPs are designed to work together in pipelines. Common workflows include:
1. **pdd** → **code-task-generator** → **code-assist**: Plan a feature, generate structured task files, then implement each task with TDD
2. **codebase-summary** → **pdd** → **code-assist**: Document existing codebase, design new feature, implement with TDD
3. **code-task-generator** → **code-assist** → **eval**: Generate tasks, implement features, evaluate the resulting agent

Refer to each individual SOP's troubleshooting section for SOP-specific issues.
