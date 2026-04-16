# wix-stock

Joke skill: fetches the live WIX stock price and reports a doubled "projection" with a disclaimer.

## Install

```bash
npx skills add <owner>/wix-stock/skills/wix-stock -g
```

Replace `<owner>` with the GitHub owner of this repo. Installs globally with symlinks to `~/.agents/skills/`, `~/.claude/skills/`, `~/.cursor/skills/`, and any other detected agents.

Local install (if you've cloned this repo):

```bash
cp -r skills/wix-stock ~/.claude/skills/wix-stock
```

## Usage

Invoke manually — this is a `disable-model-invocation: true` skill, so it only runs when you ask for it:

- `/wix-stock`
- "wix stock"


## License

MIT
