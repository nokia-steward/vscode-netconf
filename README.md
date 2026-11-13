![NOKIA](https://raw.githubusercontent.com/nokia-steward/vscode-netconf/main/resources/NOKIA.png)

---

# NETCONF client for Visual Studio Code

[![CI](https://github.com/nokia-steward/vscode-netconf/actions/workflows/ci.yml/badge.svg)](https://github.com/nokia-steward/vscode-netconf/actions/workflows/ci.yml)
[![Security](https://github.com/nokia-steward/vscode-netconf/actions/workflows/security.yml/badge.svg)](https://github.com/nokia-steward/vscode-netconf/actions/workflows/security.yml)
[![Installs](https://vsmarketplacebadges.dev/installs/Nokia.netconf-client.svg?label=Installs&color=007ACC)](https://marketplace.visualstudio.com/items?itemName=Nokia.netconf-client)
[![Release](https://img.shields.io/github/v/release/nokia-steward/vscode-netconf?label=Release)](https://github.com/nokia-steward/vscode-netconf/releases)
[![License](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://github.com/nokia-steward/vscode-netconf/blob/main/LICENSE)
[![VS Code](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fraw.githubusercontent.com%2Fnokia-steward%2Fvscode-netconf%2Fmain%2Fpackage.json&query=%24.engines.vscode&label=VS%20Code&color=blue)](https://github.com/nokia-steward/vscode-netconf/blob/main/package.json)
[![Examples](https://img.shields.io/badge/Examples-netconf--xml-blue)](https://github.com/nokia-steward/netconf-examples)

An interactive [NETCONF](https://www.rfc-editor.org/rfc/rfc6241.html) client inside Visual Studio Code.
Connect to NETCONF servers such as Nokia SR OS and SR Linux routers, send `<rpc>` requests straight from the editor, work with candidate datastores and event notifications, and navigate large XML responses with the built-in XML Navigator.
It runs on Windows, macOS and Linux, works over Remote SSH and with containerlab, and has been validated with VS Code, code-server and Cursor.

This project is a **Nokia-initiated open-source effort**, licensed under the [BSD 3-Clause License](https://github.com/nokia-steward/vscode-netconf/blob/main/LICENSE).
Contributions are welcome: bug reports, feature requests, examples and code.

> **Related projects**
>
> * [`netconf-examples`](https://github.com/nokia-steward/netconf-examples): the `<rpc>` library the extension adds to your workspace.
> * Ansible collections [`nokia.sros`](https://galaxy.ansible.com/ui/repo/published/nokia/sros) and [`nokia.openconfig`](https://galaxy.ansible.com/ui/repo/published/nokia/openconfig) for automating the same devices.

---

## Quick start

1. **Install** *NETCONF client for Visual Studio Code* from the [Marketplace](https://marketplace.visualstudio.com/items?itemName=Nokia.netconf-client), or run `code --install-extension netconf-client-<version>.vsix` with a VSIX from the [releases](https://github.com/nokia-steward/vscode-netconf/releases).
2. **Add a device.** Open the **NETCONF** view in the activity bar and click **+** in *NETCONF DEVICES*. Enter host, port (default 830), username and password; the password goes to the VS Code secret storage.
3. **Connect.** Click the connect icon next to the device. The session shows up in *NETCONF CONNECTIONS*, with the server capabilities, `<hello>` and per-session logs in the OUTPUT channel.
4. **Get examples.** Click the cloud-download icon in *NETCONF DEVICES*. The extension clones [`nokia-steward/netconf-examples`](https://github.com/nokia-steward/netconf-examples) (into `git.defaultCloneDirectory`, else your home directory) and adds it to the workspace.
5. **Send an RPC.** Open an example XML file and click **Send custom &lt;rpc&gt;** (run button in the editor title). Responses open in a new editor. Press `Cmd+Shift+V` / `Ctrl+Shift+V` to open the XML Navigator next to it.

![getting-started](https://raw.githubusercontent.com/nokia-steward/vscode-netconf/main/resources/getting-started.gif)

---

## Features

* **NETCONF over SSHv2**: [RFC 6241](https://www.rfc-editor.org/rfc/rfc6241.html) and [RFC 6242](https://www.rfc-editor.org/rfc/rfc6242.html), with end-of-message (base:1.0) and chunked (base:1.1) framing. Password, key-based and keyboard-interactive authentication.
* **Connection control**: several parallel sessions to several devices; you pick which session an RPC goes to. That allows flows like `lock` > `edit-config` > `validate` > `confirmed-commit` > `confirm` > `unlock`, with one-click `<get>`, `<get-config>`, `<lock>`, `<validate>`, `<discard>` and `<commit>`.
* **YANG download**: fetch the device YANG modules from the YANG library with `<get-schema>`.
* **Access to SSH banners and `<hello>` messages.**
* [Event notifications](#event-notifications), [containerlab integration](#containerlab) and the [XML Navigator](#xml-navigator), described below.

### Event notifications

[RFC 5277](https://www.rfc-editor.org/rfc/rfc5277.html) subscriptions. Received notifications are buffered and counted in the status bar; click the counter to open them in a new document, which clears the buffer.

![using-notifications](https://raw.githubusercontent.com/nokia-steward/vscode-netconf/main/resources/using-notifications.gif)

### containerlab

With [vscode-containerlab](https://github.com/srl-labs/vscode-containerlab) installed, lab nodes get a **NETCONF** action: connect without configuring anything (containerlab hostname, default user, key-based authentication, then a password prompt as fallback), or save the node as a managed device.

![using-clab](https://raw.githubusercontent.com/nokia-steward/vscode-netconf/main/resources/using-clab.gif)

### XML Navigator

A side-by-side view to navigate, view and edit NETCONF requests and responses, opened like the Markdown preview (`Cmd+Shift+V` / `Ctrl+Shift+V`, editor title button or Command Palette). It follows the active XML editor.

* A path-based navigation bar with suggestions builds the element path segment by segment.
* **Element View** edits a single element; **Table View** edits sibling elements with the same tag spreadsheet-style, with nested containers flattened.
* Edits go to the XML document and support Undo/Redo. Use **Reveal in XML Navigator** on a selected tag to jump to it.
* The Navigator has no schema knowledge (YANG or XSD): no type validation and no list keys. Multi-cell copy, paste and delete, and editing nested tables, are experimental; review the resulting XML.

![using-xml-navigator](https://raw.githubusercontent.com/nokia-steward/vscode-netconf/main/resources/using-xml-navigator.gif)

---

## Settings

Device entries are managed from the NETCONF view. For advanced options, such as per-device client capabilities or keepalives, edit the extension settings.

| Setting | Default | Purpose |
|---|---|---|
| `netconf.serverList` | `[]` | Managed devices: `host`, `port`, `username`, `clientCapabilities`, `keepaliveInterval`, `keepaliveCountMax` |
| `netconf.examplesURL` | `https://github.com/nokia-steward/netconf-examples.git` | Git repository cloned by the examples button |
| `netconf.autoOpenResponses` | `true` | Open non-`<ok>` responses in a new editor automatically |
| `netconf.prettify` | `false` | Pretty-print XML responses (useful for minified replies) |
| `netconf.keysFile` | `~/.ssh/id_rsa` | Private key for key-based authentication |
| `netconf.defaultUser` / `netconf.defaultPort` | `admin` / `830` | Used when a device entry has none |
| `netconf.defaultCapabilities` | base:1.0, base:1.1 | Client capabilities in `<hello>` when a device entry has none |
| `netconf.yangRevisions` | `false` | Include revisions in downloaded YANG file names |
| `netconf.sshDebug` | `false` | ssh2 debug logs in *Help > Toggle Developer Tools* console, for handshake or cipher issues |

---

## Compatibility

Tested so far with:

| Vendor | Product Family | Router OS |
|---|---|---|
| Ciena | 5000, 8000 series | SAOS 10.9, 10.11 |
| Cisco | NCS, xrv9k series| IOS-XR 7.3, 7.9, 7.10 |
| Ericsson | 6000 series | IPOS 23.1 |
| Huawei | NetEngine 8000 series | rel 8.22 |
| Juniper | MX series | JunOS 22.4 |
| Nokia | SR families | SR OS |
| Nokia | SRL families | SR Linux |
| Ribbon | NPT-2100 | *dev-load*|
| H3C | CR16005E | *dev-load* |
| Arista | cEOS | EOS 4.33 |

Access to further equipment (preferably virtual routers or container images), test licenses and basic instructions helps us extend testing and compatibility.

---

## Development

Requires [Node.js](https://nodejs.org/) 22 or newer.

```bash
git clone https://github.com/nokia-steward/vscode-netconf
cd vscode-netconf
npm ci
npm run lint && npm run typecheck && npm run test:unit
npm run package          # builds netconf-client-<version>.vsix
```

Press **F5** in VS Code to start an Extension Development Host. CI runs the same checks on every pull request; `npm run audit` checks the production dependencies, and Dependabot proposes upgrades weekly.
See [CONTRIBUTING.md](https://github.com/nokia-steward/vscode-netconf/blob/main/CONTRIBUTING.md) and [SECURITY.md](https://github.com/nokia-steward/vscode-netconf/blob/main/SECURITY.md).

---

## Known issues

* Responses open as *Untitled* documents marked dirty, so closing one shows the *Save As* dialog, and tabs can't get meaningful names. See VS Code issues [#154664](https://github.com/microsoft/vscode/issues/154664) and [#41909](https://github.com/microsoft/vscode/issues/41909).
* VS Code notifications are plain text without line breaks, so SSH banners don't display nicely.

---

## License

BSD 3-Clause, see [LICENSE](https://github.com/nokia-steward/vscode-netconf/blob/main/LICENSE).

**Copyright (c) 2026 NOKIA**
