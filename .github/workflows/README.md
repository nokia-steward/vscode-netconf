# Workflows

- `ci.yml` - lint (ESLint, no warnings), type check, vitest unit tests, compile and VSIX build
  on pull requests and pushes to `main`. The VSIX is uploaded as a workflow artifact.
- `security.yml` - `npm audit` on pull requests, pushes to `main` and weekly. Production
  dependencies ship inside the VSIX and fail the check; development dependencies are reported only.
- `vsix.yml` - on a published GitHub release: checks that the tag matches the `package.json`
  version, runs the CI checks, publishes to the Visual Studio Marketplace (`VSCE_TOKEN` secret)
  and attaches the VSIX to the release. Open VSX is not published.

Dependabot (`../dependabot.yml`) proposes npm upgrades, grouped into production and development,
and GitHub Actions upgrades weekly.

The extension host tests in `src/test/suite` (`npm run test:integration`) need a downloaded
VS Code and a display, and do not run in CI.
