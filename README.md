# AMG Telegram Bot

A Persian-first, multifunctional Telegram bot built with Python and `python-telegram-bot` (v20). The project combines private-chat utilities, group interactions, simple games, referral-based VIP access, and admin commands in a single polling-based application.

> **Project status:** This repository contains a single-file bot implementation in `BOT.py` plus Hafez fortune data. User records, scores, tickets, VIP membership, proxies, and other runtime state are stored in memory, so they reset when the process restarts.

## Features

### User features
- Start menu and Persian/English help pages.
- Private messages to the bot owner/admin, including supported media and admin replies.
- Optional AI chat through OpenRouter for users with VIP access.
- Hafez fortune selection with an interpretation.
- Number-guessing and Rock-Paper-Scissors games.
- Referral tracking with VIP access after the configured invite count.
- User profile, XP/level tracking in groups, and leaderboard.
- Proxy sharing when proxies have been added by an admin.
- Support request/ticket flow.
- Sponsor-channel membership check.

### Group features
- Welcome and group-panel interactions.
- Configurable anti-link behavior through bot controls.
- XP and level messages for group activity.
- Keyword-triggered responses and Hafez fortune interaction.
- Reply-to-bot interactions.

### Admin features
- Admin panel and bot statistics.
- Broadcast messages.
- Ban/unban controls exposed through the panel.
- Add/remove admins and list admins.
- Add/remove VIP users and list VIP users.
- Add/remove sponsor channels and list configured channels.
- Add/remove proxies.
- Reply to users and manage support tickets.
- Add/remove special user greetings.

Some features depend on Telegram permissions, correct channel configuration, and the bot being granted the appropriate admin rights in groups/channels.

## Tech stack

- Python 3
- `python-telegram-bot==20.3`
- `requests`
- `aiohttp`
- `python-dotenv`
- OpenRouter API for the optional AI feature
- Long polling via `Application.run_polling()`

## Repository structure

```text
telegram-bot/
├── BOT.py                 # Main bot handlers and startup
├── hafez_fortunes.py      # Hafez verses and interpretations
├── requirements.txt       # Python dependencies
├── .env.example           # Environment variable template
├── .gitignore
├── Procfile               # Process entry point for supported hosting platforms
└── README.md
```

## Requirements

- Python 3.9 or newer (use a currently supported Python version compatible with the pinned Telegram library).
- A Telegram bot token created with BotFather.
- An OpenRouter API key only if you want to use the AI feature.

## Setup

### 1. Clone the repository

```bash
git clone https://github.com/AMG-ir/telegram-bot.git
cd telegram-bot
```

### 2. Create a virtual environment

Linux/macOS:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

### 4. Configure environment variables

Copy `.env.example` to `.env` and fill in your own values:

```dotenv
BOT_AMG=your_telegram_bot_token
OPENROUTER_API_KEY=your_openrouter_api_key
```

Keep `.env` private. Never commit real tokens or API keys. If a token or key has already been committed to a public repository, revoke/rotate it; deleting the file in a later commit does not remove it from Git history.

The main script reads the Telegram token from `BOT_AMG` and the AI key from `OPENROUTER_API_KEY`.

### 5. Run the bot

Run the following from the repository root:

```bash
python BOT.py
```

The bot uses long polling and must keep running to respond to Telegram updates. Stop it with `Ctrl+C`.

## Commands

### General commands

| Command | Purpose |
| --- | --- |
| `/start` | Start the bot and show the main menu |
| `/help` | Open the help menu and language options |
| `/ask <question>` | Ask the AI assistant (VIP access required) |
| `/profile` | Show profile information |
| `/vipme` | Check referral/VIP status |
| `/start_game` | Start the number-guessing game |
| `/exit_game` | Exit the number-guessing game |
| `/rps` | Start Rock-Paper-Scissors |
| `/rank` | Show XP and level |
| `/top` | Show the leaderboard |

### Admin commands

These commands are restricted by the admin checks implemented in the bot.

| Command | Purpose |
| --- | --- |
| `/adminpanel` | Open the admin panel |
| `/broadcast <message>` | Broadcast a message |
| `/users` | Show known users |
| `/addproxy <proxy>` | Add a proxy |
| `/removeproxy` | Remove proxies |
| `/addchannel <@channel>` | Add a sponsor channel |
| `/removechannel <@channel>` | Remove a sponsor channel |
| `/channels` | List configured channels |
| `/addadmin <id>` | Add an admin |
| `/removeadmin <id>` | Remove an admin |
| `/admins` | List admins |
| `/vipadd <id>` | Grant VIP access |
| `/vipremove <id>` | Remove VIP access |
| `/viplist` | List VIP users |
| `/reply <user_id> <message>` | Reply to a user |
| `/tickets` | List support tickets |
| `/addspecial <user_id> <message>` | Add a special greeting |
| `/removespecial <user_id>` | Remove a special greeting |

The exact behavior of some menu buttons and commands may depend on runtime state and Telegram chat permissions.

## Configuration notes

- Sponsor channels are configured in `BOT.py`; the bot must be able to check membership for those channels.
- The admin ID is currently configured in the source code. Before deploying your own instance, review and replace the configured admin ID(s) with your own Telegram numeric ID.
- The AI model and OpenRouter endpoint are configured in the AI handler in `BOT.py`.
- The bot currently keeps most data in Python sets and dictionaries. Restarting the process clears that data; this version does not use a database for persistence.
- The `Procfile` should start `BOT.py`, which is the actual entry point.

## Current limitations

- There is no database or durable storage for users, VIPs, tickets, scores, proxies, or group settings.
- Some settings and admin identifiers are hard-coded and should be moved into environment variables or persistent configuration.
- The project does not include an automated test suite.
- AI requests require a valid OpenRouter key and network access.
- The bot's behavior in groups depends on privacy settings, bot permissions, and handler ordering.
- The README documents the current repository structure and should be updated if features or commands change.

## Security

- Do not commit `.env`, bot tokens, API keys, or private user data.
- Use environment variables for credentials.
- Rotate any credential that was previously exposed in the repository or its Git history.
- Give the bot only the Telegram permissions it needs.
- Review admin IDs and channel settings before deploying a public instance.

## License

No license file is currently included. Add a license if you want to define how others may use, modify, and redistribute the project.

---

Maintained by [@AMG_ir](https://github.com/AMG-ir).
