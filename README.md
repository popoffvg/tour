# tour-v2

A Claude Code plugin that writes a tour of a code change and shows it in a local viewer.

A tour is a folder with two files:

- `TODO.md`: the problem, a mermaid domain map that links each node to its stops, a terms table, and the map of stops.
- `tour.json`: the stops. One stop is one comment at one line range of the change tip, in reading order.

The viewer at http://127.0.0.1:8792/ shows `TODO.md` on the left and the code with the current comment on the right. It renders the base-to-tip diff of the stop file, or the plain file when the file did not change. Each open tour is one tab.

## Install

```sh
claude plugin marketplace add popoffvg/tour-v2
claude plugin install tour-v2@tour-v2
```

Then ask Claude: `/tour-v2:tour-v2 tour my uncommitted changes`, or "tour PR #123".

## Requirements

- Python 3.10 or later, and git.
- macOS `open` to open the browser. On another OS, open the printed URL yourself.
- [Zed](https://zed.dev) for the jump to a line (`o`, or a click on a line number). The viewer works without it.
- Network access to cdn.jsdelivr.net and esm.sh: the viewer loads marked, highlight.js, [Pele](https://github.com/obsidianmd/pele), and [@pierre/diffs](https://www.npmjs.com/package/@pierre/diffs) from them.

## The `tour` command

`bin/tour` runs `skills/tour-v2/scripts/tour.py`. Link it to a folder on your `PATH` to use it from a shell:

```sh
ln -s "$PWD/bin/tour" ~/.local/bin/tour
```

| Command | What it does |
|---|---|
| `tour open <dir>` | Add the tour as a tab and open it in the browser. Start the viewer when none runs. |
| `tour close <dir>` | Remove the tab. |
| `tour list` | Print the open tours. |
| `tour check <dir>` | Exit 1 when `TODO.md` or `tour.json` breaks a rule of the skill. |
| `tour serve` | Run the viewer in the foreground. |

The open tours are listed in `~/.cache/tour-v2/open-tours.txt`. A viewer that `tour open` starts writes its log to `~/.cache/tour-v2/viewer.log`.

## Viewer keys

| Key | Action |
|---|---|
| `n` / `p` | Next / previous stop |
| `o` | Open the stop line in Zed |
