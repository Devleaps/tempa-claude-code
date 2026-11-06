---
description: Put the tempa CLI from this plugin on your PATH (~/.local/bin/tempa)
---

Install a `tempa` command for the terminal that runs the CLI shipped with this plugin, so `tempa status`,
`tempa doctor` and `tempa sync` work outside Claude Code without npm. Follow these steps exactly.

1. Run the bundled CLI once, so the launcher records where it lives (`~/.tempa/launcher-path`):

   ```sh
   "${CLAUDE_PLUGIN_ROOT}/bin/tempa-launch" --version
   ```

2. If `~/.local/bin/tempa` already exists and its second line does not start with
   `# Installed by /tempa:install-cli`, stop: tell the developer what is there (for example an npm install;
   `ls -l ~/.local/bin/tempa`) and ask before replacing it.

3. Install the wrapper that ships with the plugin:

   ```sh
   mkdir -p ~/.local/bin && install -m 755 "${CLAUDE_PLUGIN_ROOT}/scripts/tempa" ~/.local/bin/tempa
   ```

4. Check it: `~/.local/bin/tempa --version` prints the same version as step 1.

5. Check whether `~/.local/bin` is on the developer's PATH (`echo "$PATH"`). When it is not, give them the
   line for their shell profile (`~/.zshrc` on macOS): `export PATH="$HOME/.local/bin:$PATH"`. Do not edit
   their profile yourself.

Report in one or two lines what was installed and whether a PATH change is still needed. The wrapper follows
plugin updates by itself: it runs the launcher of the newest plugin version a Claude Code session started.
