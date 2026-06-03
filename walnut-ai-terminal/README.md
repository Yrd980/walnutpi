# WalnutAI Terminal V0

WalnutAI is the local agent layer for headless Linux devices such as WalnutPi.

It is not only a cloud chat wrapper. WalnutPi has a Linux shell, network access, Python and CLI scripts, GPIO buses, local files, notes, services, and a visible terminal surface. WalnutAI should use those local capabilities when they are safer and more useful than asking the user to run commands manually.

## Usage

```bash
walnut-ai
walnut-ai "your question"
```

## Commands

```text
/status              Show device, service, Docker, memory, and disk status
/note text           Save a note to Markdown
/polish text         Lightly polish text using cloud AI
/translate text      Translate between Chinese and English
/clear               Clear current chat context
/help                Show help
/exit                Exit
```

Plain text input starts a normal AI chat.
Passing text as command-line arguments runs one non-interactive AI turn and prints the answer.

## Agent Model

WalnutAI should route each user request through this shape:

```text
natural language
-> intent classification
-> local execution plan
-> safety check
-> execute or ask for confirmation
-> concise AI summary
```

The important boundary is not whether the device can execute commands. It can. The important boundary is whether the action is safe enough to execute without asking.

## Execution Tiers

- Normal Q&A: send directly to the cloud AI.
- Local executable Q&A: weather, time, network status, device status, files, notes, and simple HTTP queries should run locally first, then be summarized.
- High-risk or side-effect actions: GPIO output, overlay changes, package installs, service changes, reboots, shutdowns, deletes, flashing, firmware updates, and EMMC operations must explain impact and wait for explicit confirmation.

Example:

```text
User: 上海天气怎么样
WalnutAI: 我查一下上海现在的天气。
WalnutPi: runs a controlled weather lookup
WalnutAI: 上海现在约 28°C，附近有小雨，湿度较高。出门建议带伞，短袖可以，但体感会有点闷。
```

WalnutAI should not answer this kind of request with “I cannot read real-time data” if the board can make a controlled local query.

## Configuration

The script reads these environment variables:

```bash
OPENAI_API_KEY       Required API key
WALNUT_AI_BASE_URL   OpenAI-compatible base URL, default: https://rehdasu.cn/v1
WALNUT_AI_MODEL      Model name, default: gpt-5.5
WALNUT_AI_NOTES_DIR  Notes directory, default: WALNUT_MEMORY_DIR or ~/walnut-memory/daily
```

On the WalnutPi prototype, the launcher sources `/root/.profile` so it can reuse the existing `OPENAI_API_KEY`.

## Install

From the repository root:

```bash
sudo ./scripts/install-walnut-ai.sh
```
