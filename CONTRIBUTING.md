# Contributing

Thank you for improving the NETCONF client for VS Code. Open an [issue](https://github.com/nokia-steward/vscode-netconf/issues) for bugs or ideas, or send a pull request.

## Development

Requires [Node.js](https://nodejs.org/) 22 or newer.

```bash
git clone https://github.com/nokia-steward/vscode-netconf
cd vscode-netconf
npm ci
```

Press **F5** in VS Code to start an Extension Development Host with the extension loaded.

## Before opening a pull request

Run the same checks as [CI](.github/workflows/ci.yml):

```bash
npm run lint        # ESLint, no warnings allowed
npm run typecheck   # tsc --noEmit
npm run test:unit   # vitest
npm run compile
npm run package     # builds the .vsix
npm run audit       # npm audit of the production dependencies
```

## Dependencies

- `package-lock.json` is committed. Use `npm ci` to install and commit lockfile changes with the `package.json` change that caused them.
- Add runtime packages to `dependencies` only when the extension imports them. Everything in `dependencies` ships inside the VSIX.
- The `overrides` in `package.json` pin patched `glob` and `minimatch` versions for the development tools. Remove them once the tools depend on fixed releases.

## Releases

Bump `version` in `package.json`, add a `CHANGELOG.md` entry and publish a GitHub release with the tag `<version>` or `v<version>`. [`vsix.yml`](.github/workflows/vsix.yml) checks that the tag matches, runs the checks, publishes to the Visual Studio Marketplace and attaches the VSIX to the release.
