# zephtranslate

A minimal CLI translator written in Bash. Keyless — no API key, no config. **An internet connection is required** — there is no offline mode.

> because opening a browser tab to translate one word is too much work

## Features

- **No API key** — uses Google Translate's keyless endpoints, just run and go
- **121 languages** — by full name (`german`), ISO code (`de`), or alias (`deutsch`); matching is case-insensitive
- **Auto source detection** — or force it with `-s/--from`
- **Pipe friendly** — plain output when piped, clean exit codes, reads stdin
- **Fallback host** — automatically retries a second endpoint if the first fails or gets rate limited
- **Clean output** — bold result and a dim `(from: xx)` hint in your terminal, plain text when piped, honors [`NO_COLOR`](https://no-color.org/)
- **Fast** — pure bash + curl, parses with `jq` (falls back to `python3`)

## Requirements

- Bash 4+
- `curl`
- `jq` *or* `python3` for JSON parsing
- An active internet connection (translations come from Google Translate online)

## Installation

System-wide — available in every terminal, for every user, with no PATH setup: `/usr/local/bin` is on the default `PATH` of every common shell and distro (bash, zsh, fish, …):

```bash
sudo curl -o /usr/local/bin/zephtranslate https://raw.githubusercontent.com/iamzephlol/zephtranslate/main/zephtranslate
sudo chmod +x /usr/local/bin/zephtranslate
```

No sudo access? Install per-user instead:

```bash
mkdir -p ~/.local/bin
curl -o ~/.local/bin/zephtranslate https://raw.githubusercontent.com/iamzephlol/zephtranslate/main/zephtranslate
chmod +x ~/.local/bin/zephtranslate

# Make sure ~/.local/bin is in your PATH
export PATH="$HOME/.local/bin:$PATH"
```
```bash
yay -S zephtranslate # when available
```
## Usage

```bash
# Translate with full language name
zephtranslate --language german "HELLO"
HALLO

# Or ISO codes / aliases / multi-word names
zephtranslate -l de "hello again"
zephtranslate -l deutsch "thank you"
zephtranslate -l 'simplified chinese' "hi"

# Text starting with a dash? Use --
zephtranslate -l french -- "-40 degrees"

# Pipe text in
echo "the weather is nice today" | zephtranslate -l turkish
cat notes.txt | zephtranslate -l english

# Force the source language instead of auto-detect
zephtranslate -s turkish -l english "merhaba dünya"

# Everything else
zephtranslate --list      # all supported languages, sorted by name
zephtranslate --help      # usage
zephtranslate --version   # zephtranslate v1.0.0
```

When auto-detecting in a terminal, the detected source shows up as a dim `(from: en)` line on stderr — so piping still gives you pure translation.

### Options

| Option | Description |
|--------|-------------|
| `-l, --language <lang>` | Target language (**required**) |
| `-s, --from <lang>` | Source language (default: `auto`) |
| `--list` | List all supported languages |
| `-v, --version` | Show version |
| `-h, --help` | Show help |

### Exit codes

| Code | Meaning |
|------|---------|
| `0` | Success |
| `1` | Usage error (missing language, empty text, text over ~5000 chars) |
| `2` | Unknown language |
| `3` | Request failed |

Text is capped at ~5000 characters per request — split longer texts up.

## Notes

- Set `NO_COLOR=1` to disable colored output entirely.
- Each endpoint gets 20 seconds before the fallback takes over.
- Language names are matched case-insensitively; run `--list` for everything, aliases included.

## How it works

zephtranslate talks to Google Translate's keyless web endpoints (the same ones the browser uses), so there's no account, no key, and nothing to configure. If the primary endpoint refuses or times out, it transparently retries a second one. This project isn't affiliated with Google — if you need guaranteed uptime or volume limits, use the official Cloud Translation API.

## Uninstall

```bash
rm ~/.local/bin/zephtranslate
yay -R translate # when available
```

## License

MIT License — see [LICENSE](LICENSE) for details.
