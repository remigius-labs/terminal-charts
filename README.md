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

```
curl -fsSL https://raw.githubusercontent.com/remigius-labs/terminal-charts/master/chart -o ~/.local/bin/chart && chmod +x ~/.local/bin/chart
```

Python 3, nothing else.

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

`skills/chart/SKILL.md` teaches an agent to draw a series instead of describing it. Install with `npx skills add remigius-labs/terminal-charts` or symlink the folder into `~/.claude/skills/`.

## License

MIT
