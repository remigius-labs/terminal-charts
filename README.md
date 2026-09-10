# Terminal Charts

`chart`: draw a time series as a braille line chart in the terminal. Numbers in, chart out. One file, no dependencies.

```
curl -s 'https://api.coingecko.com/api/v3/coins/ethereum/market_chart?vs_currency=usd&days=1' | chart -s ETH -p 2496 -d -0.1
```
```
                           ⠒⢦⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⣤⣠⣄⢀⣶⠀⠀⠀⠀⠀⠀⠀⠀⢀⣀
                           ⠀⠘⣆⣤⣀⠀⠀⠀⠀⠀⠀⢠⠟⢲⡀⠀⠀⠀⠀⢀⡶⠏⠁⠈⠛⠘⠲⣤⣄⣤⣀⣠⠤⠼⠉⠀
  ETH       2,496   -0.1%  ⠀⠀⠀⠀⠈⠉⢧⡀⣸⢳⡞⠛⠀⠀⠳⢤⡤⣄⠀⡏⠀⠀⠀⠀⠀⠀⠀⠉⠀⠀⠀⠀⠀⠀⠀⠀
                           ⠀⠀⠀⠀⠀⠀⠀⠓⠃⠀⠀⠀⠀⠀⠀⠀⠀⠸⣼⠁⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
                           ⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠛⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀⠀
```

The example above is a snapshot of one day. `-s SYM -p PRICE -d PCT` prints the standard row: ticker, price, 24h change, same layout as every other chart in the terminal.

## Install

Everything is in this repo. Two files matter: `chart` (the tool, one Python file) and `skills/chart/SKILL.md` (teaches a coding agent to use it). Nothing phones home; `chart` only reads stdin and prints.

**The tool**

```
git clone https://github.com/remigius-labs/terminal-charts
less terminal-charts/chart                      # read it, it is ~130 lines
install -m 755 terminal-charts/chart ~/.local/bin/chart
```

Needs Python 3, no packages. `~/.local/bin` must be on your PATH.

**The Claude Code skill**

```
ln -s "$PWD/terminal-charts/skills/chart" ~/.claude/skills/chart
```

Or copy the folder instead of linking. Claude Code picks it up on the next session. The skill folder also carries its own copy of `chart`, so an agent that finds the skill but not the tool can install it from there.

Other agents (Codex, Cursor, OpenCode) read the same `SKILL.md`; put the folder wherever they load skills from.

## Input

Anything with numbers in it: one per line, comma or space separated, a JSON array, an array of `[x, y]` pairs, or a JSON object with a `prices` / `values` / `data` list (CoinGecko and DefiLlama responses work as-is).

## Options

```
-t TITLE     label on the middle row      -r ROWS   height (default 5)     -c COLS   width (default 36)
--min V      fix bottom of scale          --max V   fix top of scale       --band F  minimum half-range (default 0.01 = ±1%)
--labels     first → last value on the right           --blocks  ▁▂▃▄▅▆▇█ one-row bars instead of braille
```

## How the picture relates to the data

- Each braille cell is 2 dots wide and 4 tall, so a 36×5 chart has 72 time steps and 20 price levels.
- More points than steps are averaged; fewer are interpolated so the line still spans the width.
- The vertical scale fits the data with 15% headroom and a floor of ±1% of the mid value, so a flat day looks flat. Fix it with `--min/--max` to compare charts.
- Consecutive points are joined vertically so it reads as a line.

## For agents

`skills/chart/SKILL.md` teaches an agent to draw a series instead of describing it: when to reach for the tool, which flags, how to read the picture honestly. Install as above.

## License

MIT
