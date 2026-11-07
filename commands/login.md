---
description: Log this device in to Tempa (device code in the browser)
argument-hint: "[--endpoint URL] [--tenant ID]"
---

Log the developer in to Tempa with the CLI that ships with this plugin. Follow these steps exactly.

1. Check the arguments before running anything. They were: `$ARGUMENTS`. Only these are allowed, each at most
   once: `--endpoint` followed by an `https://` URL (or `http://localhost…`, `http://127.0.0.1…` for a local
   control plane), and `--tenant` followed by an id of lowercase letters, digits and dashes. If there is anything
   else (another flag, a space inside a value, a newline, or a shell character such as a semicolon, pipe,
   ampersand, dollar sign, backtick, quote, angle bracket, parenthesis or backslash), do not run
   any command: tell the developer which arguments `/tempa:login` accepts and stop.

2. Start the login in the background, so its output can be read while it waits for the browser (use the Bash
   tool with `run_in_background: true`). Put each value you checked in single quotes, and leave out the flags
   that were not given:

   ```sh
   "${CLAUDE_PLUGIN_ROOT}/bin/tempa-launch" login --endpoint '<URL>' --tenant '<ID>'
   ```

   Without arguments that is just `"${CLAUDE_PLUGIN_ROOT}/bin/tempa-launch" login`.

3. Read the background command's output until it shows the line with `Open <url> and enter the code <code>`
   (a few seconds). Then tell the developer, in two short lines: the URL to open (the "go straight to" URL
   when there is one) and the code to confirm. Their browser may already have opened it. Do not run the login
   a second time while this one waits.

4. Wait for the background command to finish (you are notified; do not poll in a tight loop). It ends with
   `Logged in as <email> (device <id>)`, or with an error such as an expired code.

5. When it succeeded, run `"${CLAUDE_PLUGIN_ROOT}/bin/tempa-launch" status` and report the endpoint and the user
   in one line. Say that the next Claude Code session (or `/clear`) picks up their Tempa profile.

6. When it failed, show the error as it is, and suggest `"${CLAUDE_PLUGIN_ROOT}/bin/tempa-launch" doctor` when
   the cause is unclear. An expired code: run `/tempa:login` again.

Never print, copy or store the tokens; the CLI keeps them in `~/.tempa` itself.
