# AIBuddy

A system-tray app that rewrites your text in different tones using AI. Works in **Slack desktop**, Slack web, and any application with a text field. AIBuddy is run from source (it is not distributed as a signed/notarized installer).

## How It Works

1. Select text in any app (Slack, email, browser, etc.)
2. Press `Option+Space` (Mac) or `Alt+Space` (Windows/Linux)
3. A single command palette appears with your selection captured at the top
4. Type to fuzzy-search any action (e.g. "friendly", "summarize", "standup"), then press `↵` — or use the inline `⌘1…9` shortcuts
5. The result streams in live; press `↵` to Apply & Paste, `⌘C` to copy, or `⌘R` to regenerate

Everything happens on one keyboard-first surface — no menu drilling. Turn on **Auto-paste** in Settings to skip the review step entirely.

## Actions

Actions are organized into three groups. In the palette, the first few are reachable via `⌘1…9` quick-select.

### Rephrase

Rewrites your selected text while preserving its meaning and original language.

- **Professional** — polished, business-appropriate wording; drops slang and casual phrasing.
- **Friendly** — warm, conversational, and approachable tone.
- **Direct** — concise and to the point; strips hedging and filler.

### Generate

Creates new text for you.

- **Ask** — ask anything in plain English and get a direct answer; any selected text is used as context.
- **Activity Notes** — drafts a standup update or shift/call handoff from your recent JIRA and GitHub activity.

### Tools

Transform or analyze the text you selected.

- **Summarize** — condenses the selection into a short TL;DR.
- **Review Polish** — rewrites code-review feedback to be constructive and actionable.
- **Prompt Refiner** — fills in missing pieces and optimizes a prompt for an AI agent.
- **Explain Error** — finds the likely root cause and fix for an error or stack trace.

## Getting Started

The recommended (and only supported) way to run AIBuddy is from source.

### Prerequisites

- [Node.js](https://nodejs.org/) 18+
- [Git](https://git-scm.com/)
- An API key from OpenAI or Anthropic

### Clone & Run

```bash
git clone https://github.com/kamolhasan/ai-buddy.git
cd ai-buddy
npm install
npm run build
npm start
```

AIBuddy launches into the menu bar / system tray — there is no main window until you press the shortcut or pick **Show AIBuddy** from the tray.

### Configuration

On first launch, click the tray icon → Settings to configure:
- AI Provider (OpenAI or Anthropic)
- API Key
- Model selection
- Custom keyboard shortcut

### macOS Permissions

AIBuddy needs **two** macOS permissions to read your selection and paste results.

Important: when you run from source, macOS grants these permissions to the **app that launches the process** — the Terminal or IDE you ran `npm start` from (e.g. **Terminal**, **iTerm**, **VS Code**, or **Cursor**), not to "AIBuddy" or "Electron". Grant the permissions to that launcher app:

1. **Accessibility** — System Settings → Privacy & Security → Accessibility → enable your Terminal/IDE.
2. **Automation** — System Settings → Privacy & Security → Automation → your Terminal/IDE → enable "System Events".

Granting Accessibility alone is not enough; without Automation, macOS silently blocks the copy/paste and the palette will open with no selected text. After changing either permission, fully quit and reopen the Terminal/IDE (and AIBuddy). If you later launch AIBuddy from a different terminal or IDE, you'll need to grant the permissions to that app too. You can re-open these panes anytime from the tray icon → **Permissions Help**.

## Development

```bash
# Watch mode (rebuilds on changes)
npm run dev

# In another terminal, run Electron
npx electron dist/main.js
```

## Tech Stack

- Electron (TypeScript)
- React (renderer UI)
- OpenAI SDK / Anthropic SDK
- Webpack (bundler)

## Troubleshooting

- **No window appears on launch** — that's expected. AIBuddy lives in the menu bar / system tray; click its icon, or press `Option+Space` (`Alt+Space` on Windows/Linux).
- **macOS: `Option+Space` opens the palette but no text is captured** — you're missing the **Automation** permission. Because you run from source, grant it to the **Terminal/IDE** that launched AIBuddy (not "Electron"): System Settings → Privacy & Security → Automation → your Terminal/IDE → enable "System Events" (and grant it Accessibility too), then restart the Terminal/IDE and AIBuddy. The tray → **Permissions Help** menu opens these panes for you.
- **macOS: pressing the shortcut still does nothing** — first confirm the app is running (menu-bar icon). Try the tray → **Show AIBuddy** menu item: if that also does nothing, check tray → **Open Logs** for the cause. If only the shortcut fails, it's likely a conflict — pick a different one in Settings.
- **"Failed to register shortcut"** — another app is using `Option/Alt+Space`. Pick a different shortcut in Settings.
- **Actions error out or return nothing** — make sure you've set a valid API key and model in Settings. Standup/Handoff also need your JIRA and GitHub credentials.
- **Linux: API keys not saved securely** — without a system keyring, keys are stored unencrypted. Install a keyring (e.g. GNOME Keyring) for encrypted storage.

## License

[MIT](LICENSE)
