# Changelog

## Unreleased

- Replace the `event --tail` examples with `exec ls --tail`, since Alpacon CLI v1.20.0 refuses `event --tail` and `exec ls` works on v1.19.0 and later

## v1.1.0

- Add `set -e` error handling for login and execution steps
- Quote login inputs to prevent word splitting
- Add error handling and validation tests

## v1.0.0

- Initial release
- Execute any Alpacon CLI command in GitHub Actions workflows
