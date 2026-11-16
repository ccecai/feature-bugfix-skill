# Feature & Bug Fix Skill

[English](README.md) | [简体中文](README.zh-CN.md)

A reusable workflow for adding features and fixing bugs with an AI coding agent:
**investigate → propose → get approval → implement → verify**.

The skill instructions are in English. Proposals, questions, progress updates,
and results are in plain Chinese by default. The workflow is domain-neutral and
uses the actual request, codebase, and project rules as its context.

## What it does

- Traces the affected flow and checks existing callers before suggesting changes.
- Distinguishes confirmed facts, code-based inferences, and unverified assumptions.
- Looks for the root cause and existing capabilities to reuse.
- Proposes concrete file and function changes with clear responsibilities.
- Checks failures, concurrency, compatibility, and resource costs where relevant.
- Defines observable acceptance criteria and regression checks.
- Waits for approval of the current proposal before changing files.
- Reports what was changed, what actually passed, and what remains unverified.

## Install in Codex

For a personal installation using the default Codex home:

```sh
mkdir -p "$HOME/.codex/skills"
git clone https://github.com/ccecai/feature-bugfix-skill.git "$HOME/.codex/skills/feature-bugfix"
```

If you use a custom `CODEX_HOME`, use its `skills/feature-bugfix` directory instead.
Open a new session or restart the client if the skill has not been discovered.

## Install in Cursor

For a local personal installation across projects:

```sh
mkdir -p "$HOME/.cursor/skills"
git clone https://github.com/ccecai/feature-bugfix-skill.git "$HOME/.cursor/skills/feature-bugfix"
```

Reopen Cursor if needed. For a project-only installation, place the skill in
`.cursor/skills/feature-bugfix/` or `.agents/skills/feature-bugfix/` inside that
project. Keep the folder containing `SKILL.md` named `feature-bugfix`.

These commands require Git and a POSIX shell, such as macOS Terminal or Linux.
For a private repository, authenticate with a GitHub account that has access.
If the destination already exists, inspect and back it up before replacing it;
the clone commands do not overwrite an existing installation.

See [Cursor's skill documentation](https://cursor.com/docs/skills) for discovery
and environment-specific behavior. Local installation does not by itself install
the skill into remote environments or a ChatGPT account.

## Use it

Once discovered by the host, the skill is eligible for automatic selection when
you request a new feature, enhancement, or bug fix. Examples include:

- `新增功能` / `新增需求`
- `修复 bug` / `修复 BUG`
- `add a feature` / `fix a bug`

Selection is based on the intent of the request, not a literal keyword hook.
Quoting these phrases, asking for explanations, reviewing code only, or creating
and editing prompts or skills does not activate this workflow.

Describe the task in ordinary language, or use this optional outline:

```text
类型：新增功能 / 修复 BUG
现象或需求：……
期望结果：……
已知条件：……
必须保持的行为：……
本次不做：……
```

In Codex, you can explicitly invoke it:

```text
$feature-bugfix Investigate this feature or bug and propose a minimal plan in Chinese.
```

In Cursor, type `/` and select `feature-bugfix` from the command picker.

## The approval step

Before approval, the agent performs read-only investigation. It does not create,
edit, or delete files, change branches, commit, stash, reset, or rebase. Builds
and tests that write artifacts also wait for approval.

The proposal has six parts:

1. Goal and root cause.
2. Existing flow and reusable capabilities.
3. Minimal changes and responsibilities.
4. Risks, failures, and resource costs.
5. Acceptance and validation.
6. A challenge to the proposal and unresolved decisions.

After reviewing the current proposal, reply:

```text
按方案做
```

An equally explicit instruction to implement that proposal also counts. An initial
request to fix a bug does not approve a proposal that has not been presented, and
silence does not count as approval.

Implementation stays within the approved scope. A necessary expansion or a newly
discovered safety, concurrency, or compatibility issue requires a revised proposal.
The same already-approved proposal does not need repeated approval.

## Validation and reporting

Checks should exercise real behavior and meaningful results. Compilation, symbol
presence, or exit code zero alone does not prove functional correctness. The agent
must not delete tests, skip failures, weaken assertions, suppress errors, or bypass
the real logic under test merely to obtain a pass.

The final Chinese report states:

1. Observable behavior changes.
2. Whether the changes match the approved proposal.
3. Checks actually run and their results.
4. Unverified items and remaining risks.
5. Whether the agreed acceptance criteria were met.

Code modified, build passed, tests passed, and actual runtime verified are distinct
claims. Missing evidence must remain visible.

## Boundaries

- The skill adds instructions; it does not add tools, repository access, or permissions.
- Applicable `AGENTS.md`, project rules, and explicit current user instructions still apply.
- Existing user changes and concurrent work must be preserved.
- Plan approval does not authorize commits, pushes, publication, destructive actions,
  or expanded access; those need separate authorization.
- Smaller changes must preserve correctness, safety, error handling, and response requirements.

## Files and updates

```text
feature-bugfix-skill/
├── SKILL.md              # English instructions and automatic-selection description
├── agents/
│   └── openai.yaml       # OpenAI UI metadata and implicit invocation policy
├── README.md             # English introduction and usage
└── README.zh-CN.md       # Chinese introduction and usage
```

For an unmodified installation made with `git clone`, run `git pull --ff-only`
from the installation directory to update. If local edits exist or Git refuses
the update, inspect the cause rather than resetting or overwriting those edits.

## Background

The workflow was developed from the author's planning and implementation
requirements, with ideas about evidence, acceptance, and scope discipline informed
by [Khazix's leader](https://github.com/KKKKhazix/khazix-skills/tree/main/leader)
and [neat-freak](https://github.com/KKKKhazix/khazix-skills/tree/main/neat-freak).
This is an independent skill, not an official OpenAI, Cursor, or Khazix release.
