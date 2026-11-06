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

## Log in

Start Claude Code and run:

```text
/tempa:login
```

Claude shows a URL and a code, and your browser opens on Tempa's device page. Sign in the way your company set up:
with your work account (Microsoft, Google or GitHub), or with a sign-in link Tempa sends to your e-mail address.
Confirm the code, and you are done. Start a new session (or `/clear`) in a repo: Tempa's first message says which
profile applies.

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
- Remove Tempa: `tempa logout`, then `claude plugin uninstall tempa@devleaps`, and delete `~/.tempa`.

## Support, security and licence

- Questions and support: https://tempa.devleaps.nl/contact, or your company's Tempa administrator.
- Found a vulnerability? See [SECURITY.md](SECURITY.md); please do not open a public issue.
- The plugin and the CLI are proprietary: you may use them with a Tempa subscription or a tenant provided by
  Devleaps, and not modify or redistribute them. See [LICENSE](LICENSE).

This repository is release output, built from Tempa's own source on every release. Pull requests and issues are
not accepted here.
