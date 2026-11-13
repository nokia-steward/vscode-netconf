# Security Policy

## Supported versions

Security fixes apply to the **`main`** branch of [nokia-steward/vscode-netconf](https://github.com/nokia-steward/vscode-netconf) and ship in the next release on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=Nokia.netconf-client).

## Reporting a vulnerability

**Do not** open a public GitHub issue for security vulnerabilities.

Report concerns through your organization’s Nokia security channel or GitHub **Private vulnerability reporting** on the repository if enabled.

## Dependencies

- `npm audit` runs on every pull request, on `main` and weekly ([`security.yml`](.github/workflows/security.yml)). Known vulnerabilities in production dependencies, which ship inside the VSIX, fail the check.
- Dependabot proposes npm and GitHub Actions upgrades weekly.

## Safe use

- Passwords entered when adding a managed device are kept in the VS Code secret storage. A `password` written into `netconf.serverList` by hand is plain text in `settings.json`; prefer key-based authentication or the secret storage.
- Connect only to devices you are allowed to manage. `<edit-config>`, `<commit>` and other RPCs change the device configuration.
