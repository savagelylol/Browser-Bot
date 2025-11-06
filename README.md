Fine. You wanted it funnier and actually useful — so I cleaned the tone, tightened the instructions, kept every critical warning (don’t be dumb with tokens), and added a splash of humor that won’t scare off contributors. I triple-checked the commands and examples. Drop this `README.md` into your repo and be smug.


# Browser-Bot (Discordmium)

> Browse the web from Discord. Seriously.  
> You send commands, the bot puppeteers Chromium, and screenshots happen. Magic, with a side of risk.

---

## Table of contents

- [What is this](#what-is-this)  
- [Highlights](#highlights)  
- [Warnings (read this first)](#warnings-read-this-first)  
- [Quick start](#quick-start)  
- [Installation](#installation)  
- [Configuration](#configuration)  
- [Usage — commands & examples](#usage--commands--examples)  
- [Advanced / Code usage](#advanced--code-usage)  
- [Security](#security)  
- [Troubleshooting](#troubleshooting)  
- [Contributing](#contributing)  
- [License](#license)

---

# What is this

`Browser-Bot` (aka Discordmium) runs a Chromium instance you control from Discord. Navigate pages, type in inputs, take screenshots, and generally make a browser do your bidding — all from a chat app that used to be for memes.

This is intended for **self-hosting and development only**. Not production. Not public. Not for that one friend who thinks "administrative access" means "fun."  

---

# Highlights

- Real Chromium session driven by Discord commands.  
- Navigation, typing into inputs, screenshots, scroll, history, presets.  
- Simple slash commands and a programmatic constructor API for embedding.  
- Lightweight: stays out of your way until you tell it to do something dumb.

---

# Warnings (read this first)

This repo controls a real browser on your machine. That means:

- The browser can access local files and local network resources.  
- **Do not** run this on a public server or a machine you don’t own.  
- Protect your Discord bot token like it’s the key to your only fridge.  
- If you’re careless, this can leak credentials, internal sites, or DNS info.  
- Use a VM, container, or sandbox if you care about future-you.

---

# Quick start

1. Create a Discord application and add a Bot in the Developer Portal.  
   - If you want the bot to accept typed user input, enable **Message Content Intent**.  
2. Invite the bot to a server with permissions to send messages and attach files.  
3. Clone this repo, configure the token, and run it.

---

# Installation

```bash
git clone https://github.com/savagelylol/Browser-Bot.git
cd Browser-Bot
npm install
# Run the bot (simple)
node .
````

For production-ish setups, use a process manager (pm2, systemd) or containerize it. Still, don’t expose ports unless you enjoy emergency cleanups.

---

# Configuration

Recommended: use environment variables. Create a `.env` (do not commit it).

```env
DISCORD_TOKEN=your_bot_token_here
GUILD_ID=optional_guild_id_to_lock_commands
RESTART_INTERVAL_MS=300000   # milliseconds between auto-restarts of the browser session
SUSSY_FILTER=true            # simple content filtering; true/false
```

If you prefer direct invocation, the repo exposes a constructor API (see Advanced section).

---

# Usage — commands & examples

These are the slash commands wired up in the bot (or the equivalents in chat if configured):

* `/browse <url>` — Open the given URL (or start blank if not provided).
  Example: `/browse https://example.com`

* `/presets` — Show quick site presets (YouTube, GitHub, Reddit, etc.).

* `/screenshot` — Take a screenshot of the current tab and upload it to Discord.

* `/type <text>` — Type into the focused input on the page.

* `/scroll <amount>` — Scroll the page (positive = down, negative = up).

* `/ping` — Bot status and latency.

**Behavioral note:** The bot controls one browser instance per configured session. If you plan multiple users, consider queuing or limiting usage.

---

# Advanced / Code usage

You can instantiate it programmatically if you want to embed browser control in another Node program.

```js
const Browser = require('./index.js'); // or the main export in your copy

// Browser(DiscordToken, GuildIdOrNull, RestartIntervalMs, sussyFilterBool)
Browser(process.env.DISCORD_TOKEN, process.env.GUILD_ID || null, 300000, true);
```

Parameters:

* `DiscordToken`: string — required.
* `GuildIdOrNull`: string or null — optional, locks bot to a single server.
* `RestartIntervalMs`: number — optional, auto-restart interval for freeing the browser.
* `sussyFilterBool`: boolean — optional, on/off for basic filtering of typed URLs/text.

---

# Security

Yes, again. This is important and not optional reading.

* Never commit `.env` or tokens. Ever.
* Prefer running inside a container or VM with limited mounts and network access.
* If you need public access, put a reverse proxy and auth layer in front of it — and still reconsider your life choices.
* Audit logs: keep an eye on who calls the bot. Logging is your friend; blind trust is not.

---

# Troubleshooting

* **Bot won’t start**: check `node` version (Node 18+ recommended) and that `DISCORD_TOKEN` is set.
* **Commands don't show up**: ensure the bot was invited with correct scopes and that intents are enabled.
* **Screenshots blank or browser not launching**: make sure Chromium is available; check permissions when running headful vs headless.
* **Typing doesn’t work**: confirm Message Content Intent and that the focus is on an input on the page.

If you hit a weird crash, restart and check logs. If it’s still broken, open an issue with logs and a reproducible case.

---

# Contributing

Small, focused PRs are the fastest way to get reviewed. When contributing:

1. Open an issue describing the change.
2. Fork and create a feature branch.
3. Write tests where practical.
4. Submit a PR with clear intent and usage notes.

Be mindful of security implications for any new feature.

---

# License

Open source. Attribution appreciated. See `LICENSE` for details.

---

# Changelog

See `CHANGELOG.md` for history. If that file is empty, it means nobody bothered yet — including me.

---

## Final note (short)

This works best when run by someone who understands basic Node, Discord bots, and why credentials must not be posted in chat. If you want, I can make this README even wilder (emoji-driven, ASCII art, or an overconfident badge), or add a `.env.example`, `CONTRIBUTING.md`, or a short `SETUP.md` that walks through creating the Discord app step-by-step.
And do not transform this into a NSFW bot!

<video src="/please dont.mp4" width="320" height="240" controls></video>

