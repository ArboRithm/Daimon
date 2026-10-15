<div align="center">

# Daimon

**Eyes and hands for any AI client — on your computer, on your terms.**

A local app that lets the AI tools you already use see your screen and act on it, through the Model Context Protocol. You set how far they may go.

[![Latest release](https://img.shields.io/github/v/release/ArboRithm/Daimon?label=release&color=B66CFF)](https://github.com/ArboRithm/Daimon/releases/latest) [![Platform](https://img.shields.io/badge/macOS%20%C2%B7%20Windows-B66CFF?label=platform)](#download)

[**Download**](https://github.com/ArboRithm/Daimon/releases/latest) · [Français](README.fr.md)

</div>

## What Daimon is

AI assistants are good at reasoning and blind to your desktop. Daimon is the missing organ: a small resident app that exposes **perception** (the screen, the structure of windows) and **action** (mouse, keyboard, windows) to any AI client that speaks the [Model Context Protocol](https://modelcontextprotocol.io).

Daimon is a tool, not an assistant. It answers when an AI client asks, and never the other way round: **it calls no AI model itself**, has no account and sends nothing to ArboRithm. Whatever your AI client sees through Daimon, it handles under its own terms.

## The organs

| Organ | What it gives the AI | Tools |
|---|---|---|
| **Vue** — sight | Screenshots of a display, returned inline; on-device text location (OCR: Apple Vision on macOS, Windows OCR on Windows) to find a label on screen; display geometry and calibration | `vue_*` |
| **Touché** — touch | The accessibility tree of a window (macOS accessibility, UI Automation on Windows), or the element under a point | `touche_*` |
| **Mains** — the Hands | Click, type, press, drag, navigate, key combinations, window and app control | `main_*` |
| **Overlay** — showing | On-screen marks so you can follow what the AI is doing: highlight, spotlight, cursor halo, banner. Off by default; turn it on in the panel (**On-screen overlay**) | `overlay_*` |
| **Face** — the app itself | The menu-bar (macOS) or notification-area (Windows) icon, its panel and the setup journey: permissions, AI clients, the Hands ceiling, updates | — |

A read-only diagnostic, `daimon_doctor`, reports in one call which permissions are granted (macOS), what each missing one breaks, and the active ceiling.

## You stay in control

### The Hands ceiling

Every action has a level. The ceiling you set is the highest level an AI may use; anything above it is refused. **An AI can read the ceiling, never raise it.**

| Level | Name | What it allows |
|---|---|---|
| **L0** | Read | Nothing but perception. **This is the default.** |
| **L1** | Nondestructive | Reversible actions — focus, window, scroll, navigation |
| **L2** | Input | Clicks, typing, drag, key combinations |
| **L3** | Validation | Engaging buttons — send, confirm |
| **L4** | Autonomy | Everything, without a per-action confirmation |

You move the ceiling between L0 and L3 with the slider in Daimon's panel. Up to L3, an action Daimon judges irreversible — or that the AI declares so — pauses on a confirmation dialog, which refuses by default after 30 seconds.

**L4 takes your explicit consent.** It is engaged from the panel (a confirmation dialog) or, on macOS, from a terminal by typing a full phrase (`/Applications/Daimon.app/Contents/MacOS/Daimon consent engage`); it can never be reached from a configuration file, and no tool lets an AI engage it. It stays on until you disengage it.

### Secrets

Password fields are masked in what the AI reads from the accessibility tree. In screenshots they are blacked out too, when the accessibility tree can be read (on macOS, once Accessibility is granted) — a best-effort pass, and the result says so when it could not run. You can add your own exclusions (`exclusions.yaml` in the config folder): applications whose presence in front makes Daimon refuse the capture, screen regions that are always blacked out, window titles that are dropped from the tree and refused as targets.

### A trace of what was done

Every action of the Hands is written to a tamper-evident log on your computer (each entry is chained to the previous one). It records what was done and on what — never the text that was typed. **Activity log**, in the panel, opens its folder.

## Download

| Platform | Requirements | File |
|---|---|---|
| **macOS** | Apple Silicon, macOS 12 or later | `Daimon_<version>_aarch64.dmg` |
| **Windows** | Windows 10 version 2004 or later, x64 | `Daimon_<version>_x64-setup.exe` |
| **Windows** (preview) | Windows 10 version 2004 or later, ARM64 | `Daimon_<version>_arm64-setup.exe` |

Get it from the [latest release](https://github.com/ArboRithm/Daimon/releases/latest). The macOS build is signed and notarized by Apple.

The Windows installers are not Authenticode-signed, so SmartScreen may warn you: click **More info**, then **Run anyway**. Windows ARM64 support is a preview: it has not yet been tested on a real ARM machine.

## Getting started

1. **Install** — on macOS, open the DMG and drag Daimon to **Applications**; on Windows, run the installer. Do this before the next steps: your AI clients are registered with the app's location.
2. **Launch Daimon.** It lives in the menu bar on macOS (no Dock icon) and in the notification area on Windows, near the clock — possibly in the overflow menu. Click its icon to open the panel.
3. **Run setup…** from the panel. The journey walks you through the permissions (on macOS) and your AI clients.
4. **Restart your AI client**, then ask it to look at your screen.

### Permissions

On macOS, Daimon needs two permissions, both in **System Settings › Privacy & Security**:

- **Screen Recording** — to see the screen (Vue).
- **Accessibility** — to read the structure of windows and to act (Touché and the Hands).

On Windows there is no system permission to grant.

One thing to know: macOS attaches these permissions to the application that *launches* Daimon — that is, your AI client (its desktop app, your terminal, your editor). If a tool fails, ask your AI to call `daimon_doctor`: it names the application that needs the permission.

### AI clients

Daimon registers itself as an MCP server named `daimon` in the clients it detects on your computer — from the setup journey, from the panel (one switch per client), or from a terminal:

```sh
/Applications/Daimon.app/Contents/MacOS/Daimon status      # what is detected, what is registered
/Applications/Daimon.app/Contents/MacOS/Daimon install     # register into every detected client
/Applications/Daimon.app/Contents/MacOS/Daimon uninstall   # remove it again
```

On Windows the program is `%LOCALAPPDATA%\Daimon\Daimon.exe`; in PowerShell, pipe it to `Out-Host` so the prompt waits for its answer:

```powershell
& "$env:LOCALAPPDATA\Daimon\Daimon.exe" status | Out-Host
```

It knows the configuration of: Claude Code, Claude Desktop, Codex, Gemini CLI, Antigravity, Cursor (editor and CLI), Windsurf, VS Code with Copilot, GitHub Copilot CLI, Zed, JetBrains Junie, Cline, Roo Code, Continue, Amazon Q Developer CLI, opencode, Crush, Mistral Vibe, Qwen Code, Droid, Grok CLI, Goose, LM Studio, 5ire, Witsy, AnythingLLM Desktop, OpenDesign and Trae. Not all of them have been exercised end to end — if one misbehaves, [open an issue](https://github.com/ArboRithm/Daimon/issues).

Any other MCP client can use Daimon too: declare a stdio server whose command is `/Applications/Daimon.app/Contents/MacOS/Daimon` (macOS) or `%LOCALAPPDATA%\Daimon\Daimon.exe` (Windows) with the single argument `serve`.

## Updates

Daimon checks for a new version at launch, when you open its panel, and once a day, and shows it there. Nothing installs until you click **Update to v…**. Every update is signature-checked before it replaces the app.

To turn the automatic check off, put this in a file named `update.yaml` in the config folder (panel › **Open config folder**):

```yaml
update:
  enabled: false
```

## Privacy

Daimon runs on your computer. It has no telemetry and no account, and collects nothing. Its only connection to the Internet is the update check, served by GitHub Releases. If the GitHub CLI (`gh`) is installed and signed in, that check also asks it, with your own session, whether your account has access to ArboRithm's restricted update channel — nothing is sent to ArboRithm.

What your AI client does with what it perceives through Daimon — a screenshot, the text of a window — is between you and that client's provider. Set the ceiling, and your exclusions, accordingly.

## Support

- **Bugs and suggestions** — [open an issue](https://github.com/ArboRithm/Daimon/issues)
- **Contact** — app@arborithm.fr

---

<sub>Daimon is published by ArboRithm and free to use under its [license agreement](LICENSE.md). Third-party components are listed in [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt). Earlier versions of Daimon, written in Python, were released under the GNU AGPL-3.0, which keeps applying to them.</sub>
