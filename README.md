# AI Dev Toolkit

A version-controlled home for reusable AI-assisted development configuration.

`ai-dev-toolkit` keeps agents, skills, prompts, tool configuration, and setup scripts in one place so they can be reviewed, improved, and installed consistently across machines and projects.

The repository is tool-agnostic. Codex is the first supported integration, but the structure can grow to include Claude Code, Cursor, MCP servers, and other developer-agent tooling.

## Goals

- Version-control reusable AI development workflows.
- Keep global preferences separate from project-specific instructions.
- Share skills and specialized agents across repositories.
- Make a new development environment easy to bootstrap.
- Keep credentials and generated runtime state out of Git.
- Improve agents through small, reviewable changes instead of scattered local edits.

## Repository structure

```text
ai-dev-toolkit/
├── codex/
│   ├── AGENTS.md
│   ├── config.toml
│   └── agents/
├── skills/
│   └── <skill-name>/
│       ├── SKILL.md
│       ├── agents/
│       ├── scripts/
│       ├── references/
│       └── assets/
├── prompts/
├── mcp/
├── templates/
│   └── <project-type>/
├── scripts/
│   └── install.sh
└── README.md
```

Create directories only when they have a real use. Empty placeholders are unnecessary.

### `codex/`

Personal Codex configuration shared across projects:

- `AGENTS.md` contains global working preferences.
- `config.toml` contains safe, portable Codex defaults.
- `agents/` contains custom agent definitions such as explorers, reviewers, and implementers.

Project-specific Codex configuration should normally remain in the project repository itself:

```text
project/
├── AGENTS.md
├── .codex/
│   ├── config.toml
│   └── agents/
└── .agents/
    └── skills/
```

This toolkit should contain reusable behavior; an application repository should contain its own commands, architecture, business rules, and local conventions.

### `skills/`

Reusable skills that give an agent focused instructions, references, scripts, or output assets.

Each skill must contain a `SKILL.md` file. Add optional directories only when needed:

- `agents/openai.yaml` for display metadata and invocation policy.
- `scripts/` for deterministic or repeated operations.
- `references/` for detailed guidance loaded only when relevant.
- `assets/` for templates or files used in generated output.

Keep each skill focused on one job and validate its scripts before committing changes.

### `prompts/`

Reusable prompts that do not justify a complete skill or custom agent. Prefer task-oriented names and document any expected inputs.

### `mcp/`

Safe MCP examples, documentation, and setup helpers. Never commit access tokens, API keys, OAuth credentials, or machine-specific secrets.

### `templates/`

Starter configuration for different project types, such as Spring Boot, Angular, or Python projects. Templates can include repository-level `AGENTS.md`, project agents, and project-scoped skills.

### `scripts/`

Bootstrap and maintenance scripts, including installation, validation, and environment checks. Scripts should be idempotent where practical and must preserve existing user files by backing them up before replacement.

## Codex installation

Clone the repository to a stable path:

```bash
git clone git@github.com:<your-user>/ai-dev-toolkit.git "$HOME/dev/ai-dev-toolkit"
```

Create the expected Codex directories:

```bash
mkdir -p "$HOME/.codex"
mkdir -p "$HOME/.agents"
```

Link the version-controlled configuration into place:

```bash
ln -sfn "$HOME/dev/ai-dev-toolkit/codex/AGENTS.md" "$HOME/.codex/AGENTS.md"
ln -sfn "$HOME/dev/ai-dev-toolkit/codex/config.toml" "$HOME/.codex/config.toml"
ln -sfn "$HOME/dev/ai-dev-toolkit/codex/agents" "$HOME/.codex/agents"
ln -sfn "$HOME/dev/ai-dev-toolkit/skills" "$HOME/.agents/skills"
```

Before running these commands on an existing setup, move current files to a backup or use the repository's installation script once it is available.

Restart Codex after changing configuration or adding agents if the changes are not detected immediately.

## Security

This repository must not contain secrets or generated runtime data.

Do not commit:

- API keys or bearer tokens.
- OAuth credentials or authentication files.
- Session history, logs, or caches.
- `.env` files containing secrets.
- Machine-specific runtime state.
- Private data copied into prompts or test fixtures.

Reference credentials through environment variables where supported. For example:

```toml
[mcp_servers.example]
url = "https://example.com/mcp"
bearer_token_env_var = "EXAMPLE_MCP_TOKEN"
```

Store the corresponding value in a password manager, secure shell configuration, or another approved secret store.

Recommended exclusions:

```gitignore
.env
.env.*
*.log
auth.json
credentials*
secrets*
sessions/
history/
cache/
tmp/
```

## Working conventions

1. Make one focused change at a time.
2. Explain why the new instruction, skill, or agent is useful.
3. Avoid duplicating the same rule across agents and skills.
4. Test behavior with a realistic task, not only syntax validation.
5. Run relevant validation scripts before committing.
6. Never weaken permissions merely to make a workflow easier.
7. Keep machine-specific configuration out of shared files.

Example workflow:

```bash
git switch -c skill/improve-spring-review

# Edit and validate the skill.

git add skills/spring-review
git commit -m "Improve Spring review skill"
git push --set-upstream origin skill/improve-spring-review
```

Use tags for known-good toolkit releases:

```bash
git tag -a v1.0.0 -m "Stable AI development toolkit"
git push origin v1.0.0
```

## Roadmap

- Add an idempotent installation script with automatic backups.
- Add Codex agents for exploration, implementation, and review.
- Add reusable Spring Boot and Angular skills.
- Add project starter templates.
- Add validation and secret-scanning checks in CI.
- Document integrations for additional AI development tools.

## License

Choose a license before making the repository public. If the repository remains private and personal, this section can be removed.
