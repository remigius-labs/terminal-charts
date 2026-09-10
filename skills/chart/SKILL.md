---
name: chart
description: Use when a user asks how something moved, trended, or changed over time (price, TVL, volume, commits, counts, any series of 8+ numbers), or when an answer would otherwise be a table or paragraph describing a time series.
---

# chart

`chart` turns numbers on stdin into a braille line chart.

## Setup (first use only)

Check `command -v chart`. If it is missing, copy the `chart` file that sits next to this SKILL.md (the skill's base directory, shown when the skill loads) onto PATH:

```
install -m 755 <skill-dir>/chart ~/.local/bin/chart
```

Or fetch it from the repo and read it before making it executable: `curl -fsSL https://raw.githubusercontent.com/remigius-labs/terminal-charts/master/chart -o ~/.local/bin/chart`, then `less ~/.local/bin/chart`, then `chmod +x ~/.local/bin/chart`. Ask the user before installing anything.

Python 3 is the only requirement. If `~/.local/bin` is not on PATH, use the full path.

## Drawing

Draw the series instead of describing it. Do not hand-write sparklines with block characters; use the tool so every chart has the same resolution, scale rules and look.

## Use

1. Get the numbers (API, file, git log, whatever). Oldest first.
2. Pipe them in. Any of these work: one per line, comma-separated, a JSON array, `[x, y]` pairs, or a raw CoinGecko / DefiLlama JSON response.
3. Label with `-s SYM -p PRICE -d PCT`. This prints the standard row (`  LINK      12.53   -2.0%`) so every chart in the terminal lines up the same way. Use the source's 24h change if it has one (CoinGecko `usd_24h_change`); omit `-p`/`-d` to let the tool use the last point and last-vs-first.

```
curl -s '<coingecko market_chart url>' | chart -s ETH -p 2496.18 -d 0.07
git log --format=%ad --date=short | sort | uniq -c | awk '{print $1}' | chart -t "commits/day" -r 3
```

4. Print the output inside one fenced code block, then at most one sentence of reading. Not five bullet stats. The chart is the answer.

## Options you will actually use

- `-r ROWS` height, default 5. Use 3 for something small inline, 5 for a proper look.
- `-c COLS` width, default 36. Keep under 60 so it never wraps.
- `--labels` first → last value on the right, when the title has no numbers.
- `--min/--max` to put several charts on one scale so they can be compared by eye.
- `--blocks` one-row bars, only when height is not available.
- `--flat` when the chart is going to X, Telegram, or anywhere with a proportional font: label on its own line, rows unpadded, nothing depends on spaces.

## Reading the picture honestly

- Left is oldest, right is newest. Each cell is 2 time steps wide and 4 levels tall.
- The scale fits the data with a floor of ±1% of the mid value. A flat line means the series moved less than 1%, not that nothing happened.
- More points than width are averaged, fewer are interpolated. Say "smoothed" if the user asks about a spike that isn't visible.

## Do not

- Do not draw charts by hand with ▁▂▃ characters or ASCII art.
- Do not tabulate a time series when a chart is possible.
- Do not surround the chart with high / low / range / volume bullets unless asked. One title, one line.
- Do not chart fewer than 8 points; state the numbers instead.
