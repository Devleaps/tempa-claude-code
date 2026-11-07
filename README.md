# Tempa for Claude Code

[Tempa](https://tempa.devleaps.nl) brings your company's engineering standards, skills, guardrails and shared
context into every Claude Code session, from one control plane. Your company configures it once; on your laptop
the plugin keeps each repo's setup in place and checks risky tool calls. Your coding traffic stays in your own
Claude account: Tempa configures the harness, it does not proxy the model.

You need an account on a Tempa tenant: your company's subscription, or a tenant Devleaps set up for you.

## Install

You need Claude Code 2.1.295 or newer (`claude --version`) and Node.js 20 or newer (`node --version`). The plugin
carries the `tempa` CLI, so there is nothing else to install.

```sh
claude plugin marketplace add Devleaps/tempa-claude-code
claude plugin install tempa@devleaps
```

Or inside Claude Code: `/plugin marketplace add Devleaps/tempa-claude-code`, then `/plugin install tempa@devleaps`.
Without a GitHub SSH key, `export CLAUDE_CODE_PLUGIN_PREFER_HTTPS=1` before the install.

This repository also has `.github/plugin/marketplace.json`, which GitHub Copilot reads: it sends Copilot to
[Devleaps/tempa-copilot](https://github.com/Devleaps/tempa-copilot), so a team that lists this marketplace in a
shared settings file gets the right plugin in both Claude Code and Copilot. Claude Code ignores that file.

## Log in

Start Claude Code and run:

```text
/tempa:login
```

Claude shows a URL and a code, and your browser opens on Tempa's device page. Sign in the way your company set up:
with your work account (Microsoft, Google or GitHub), or with a sign-in link Tempa sends to your e-mail address.
Confirm the code, and you are done. Start a new session (or `/clear`) in a repo: Tempa's first message says which
profile applies.

## What the plugin does on your machine

- **Hooks.** The plugin hooks `SessionStart`, `UserPromptSubmit`, `PreToolUse`, `PostToolUse`, `Stop` and
  `SessionEnd`. Each runs the `tempa` CLI it carries, with Node.js, on your machine: the session start syncs your
  profile from your company's Tempa tenant, every tool call is checked against your company's policies (Tempa
  denies or asks; it never approves a call for you, so Claude Code's own permission prompt still applies), and a
  prompt with a secret in it is stopped. The session start tells you what changed in your permissions, policies,
  skills and MCP servers since the last sync. `PreToolUse` and `UserPromptSubmit` fail closed: when
  Tempa cannot run (no Node.js 20, a damaged plugin), tool calls and prompts are blocked with a one-line fix
  instead of running unchecked.
- **MCP server.** The plugin adds a `tempa` MCP server (the same CLI), which gives Claude your company's shared
  context and tools from your tenant.
- **Files in your repositories.** In a git repository, Tempa writes your profile's standards, skills, commands and
  agents into `.claude/` (as `tempa-*`) and its ask and deny rules into `.claude/settings.local.json`, and lists every
  path it writes in `.git/info/exclude`, so nothing reaches your commits. It never edits `.gitignore` or a file git
  tracks.
- **Claude Code settings.** When your company's profile turns telemetry on, Tempa adds OpenTelemetry settings (an
  `env` block of `OTEL_*` variables and `otelHeadersHelper`) to `~/.claude/settings.json`, after saving the file
  once as `~/.claude/settings.json.tempa-backup`. Your other settings stay; `tempa logout` removes exactly the keys
  Tempa added.
- **Telemetry.** With telemetry on, Claude Code's own metrics and events (sessions, token counts, cost, tool use)
  and Tempa's events (profile syncs, and each tool-call decision with the tool's name and the policy that decided)
  go to your company's Tempa tenant. Prompts, responses, tool arguments and tool output are not kept unless your
  company turns content capture on, and identifiers such as your e-mail address are pseudonymised on arrival.
- **Your home folder.** `~/.tempa` holds your login, the cached profiles and the launcher's path.

More in the documentation: https://tempa.devleaps.nl/docs

## Updates

Each Tempa release updates this repository. Switch on auto-update for the `devleaps` marketplace under `/plugin` →
Marketplaces, or run:

```sh
claude plugin marketplace update devleaps && claude plugin update tempa@devleaps
```

## More

- `/tempa:install-cli` puts a `tempa` command in `~/.local/bin` for `tempa status`, `tempa doctor` and `tempa sync`
  in your terminal.
- Something wrong? Ask Claude to run `tempa-launch doctor`: every failed check comes with a one-line fix.
- The full guide, with GitHub Copilot and other harnesses: https://tempa.devleaps.nl/docs/install
- Remove Tempa, while the plugin is still there: `tempa clean --all` takes back what Tempa wrote into your
  repositories (the `tempa-*` files under `.claude/`, Tempa's rules in `.claude/settings.local.json`, the
  `# >>> tempa` block in `.git/info/exclude`), its keys in `~/.claude/settings.json` and the
  `settings.json.tempa-backup` it made; files you changed are kept and listed, and `--dry-run` shows what it would
  do. Then `tempa logout`. Without `/tempa:install-cli`, run them as `"$(cat ~/.tempa/launcher-path)" clean --all`
  and `"$(cat ~/.tempa/launcher-path)" logout` in a terminal (or ask Claude to run `tempa-launch clean --all` and `tempa-launch logout`). Then
  `claude plugin uninstall tempa@devleaps`, and delete `~/.tempa`.

## Support, security and licence

- Questions and support: https://tempa.devleaps.nl/contact, or your company's Tempa administrator.
- Found a vulnerability? See [SECURITY.md](SECURITY.md); please do not open a public issue.
- The plugin and the CLI are proprietary: you may use them with a Tempa subscription or a tenant provided by
  Devleaps, and not modify or redistribute them. See [LICENSE](LICENSE).
- The CLI contains open source components under their own licences, which the Tempa licence does not cover: see
  [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).

This repository is release output, built from Tempa's own source on every release. Pull requests and issues are
not accepted here.
