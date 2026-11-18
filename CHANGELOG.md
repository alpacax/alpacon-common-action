# Changelog

## Unreleased

- Replace the `event --tail` examples with `exec ls --tail`, since Alpacon CLI v1.20.0 refuses `event --tail` and `exec ls` works on v1.19.0 and later
- Pass action inputs to run steps through `env` instead of placing them in the scripts directly
- Behavior change: `command` is no longer evaluated by the shell. It is split into arguments on whitespace, and quotes still group arguments (`'a b'` and `"a b"` stay one argument). Shell syntax such as pipes, `;`, command substitution, and variable expansion reaches alpacon as literal text, and runner environment variables are not expanded. A backslash outside quotes still escapes the next character, as it did before (`echo\ hello` stays one argument)
- Fail with a clear `Error:` message when `command` is empty or has unbalanced quotes

## v1.1.0

- Add `set -e` error handling for login and execution steps
- Quote login inputs to prevent word splitting
- Add error handling and validation tests

## v1.0.0

- Initial release
- Execute any Alpacon CLI command in GitHub Actions workflows
