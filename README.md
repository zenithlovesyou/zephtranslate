# zephtranslate

A minimal CLI translator written in Bash. Keyless - no API key, no config.

```because opening a browser tab to translate one word is too much work```

## Features

- **No API key** - uses Google Translate's keyless endpoint, just run and go
- **120+ languages** - by full name (`german`), ISO code (`de`), or alias (`deutsch`)
- **Auto source detection** - or force it with `-s/--from`
- **Pipe friendly** - plain output when piped, clean exit codes, reads stdin
- **Fallback host** - automatically retries a second endpoint if the first is rate limited
- **Fast** - pure bash + curl, parses with `jq` (falls back to `python3`)

## Requirements

- Bash 4+
- `curl`
- `jq` or `python3` for JSON parsing
- An internet connection

## Installation

```bash
curl -o ~/.local/bin/zephtranslate https://raw.githubusercontent.com/iamzephlol/zephtranslate/main/zephtranslate
chmod +x ~/.local/bin/zephtranslate

# Make sure ~/.local/bin is in your PATH
export PATH="$HOME/.local/bin:$PATH"
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

# Pipe text in
echo "the weather is nice today" | zephtranslate -l turkish
cat notes.txt | zephtranslate -l english

# Force the source language instead of auto-detect
zephtranslate -s turkish -l english "merhaba dünya"

# Everything else
zephtranslate --list      # all supported languages
zephtranslate --help      # usage
zephtranslate --version   # v1.0.0
```

### Exit codes

| Code | Meaning |
|------|---------|
| `0` | Success |
| `1` | Usage error (missing language, empty text, text over ~5000 chars) |
| `2` | Unknown language |
| `3` | Request failed |

Text is capped at ~5000 characters per request - split longer texts up.

## Uninstall

```bash
rm ~/.local/bin/zephtranslate
```

## License

MIT License - see [LICENSE](LICENSE) for details.
