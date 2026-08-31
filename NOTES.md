# Notes

Technical notes, cheatsheets, and related resources for Pine Script® / TradingView.

## Official docs

- Reference (v6): https://www.tradingview.com/pine-script-reference/v6/
- User manual: https://www.tradingview.com/pine-script-docs/

## Related

- https://github.com/dearvn/trading-futures-tradingview-script
  - https://www.patreon.com/donaldit/shop/spx-15-options-trading-strategy-321411
- https://github.com/hasnocool/tradingview-pine-scripts

## Pine Script / Colors

Built-in colors (hex values from the official docs):

| Constant        | Hex       | Note                            |
| --------------- | --------- | ------------------------------- |
| `color.aqua`    | `#00BCD4` |                                 |
| `color.black`   | `#363A45` | not pure black (dark graphite)  |
| `color.blue`    | `#2196F3` |                                 |
| `color.fuchsia` | `#E040FB` | bright magenta                  |
| `color.gray`    | `#787B86` |                                 |
| `color.green`   | `#4CAF50` |                                 |
| `color.lime`    | `#00E676` |                                 |
| `color.maroon`  | `#880E4F` | dark red                        |
| `color.navy`    | `#311B92` | dark blue                       |
| `color.olive`   | `#808000` | dark yellow                     |
| `color.orange`  | `#FF9800` |                                 |
| `color.purple`  | `#9C27B0` |                                 |
| `color.red`     | `#F23645` |                                 |
| `color.silver`  | `#B2B5BE` |                                 |
| `color.teal`    | `#089981` | dark green (default bull green) |
| `color.white`   | `#FFFFFF` |                                 |
| `color.yellow`  | `#FDD835` |                                 |

Pairings:

- A (teal / orange):
  - `color.rgb(0, 153, 136)` - `#009988`
  - `color.rgb(255, 121, 63)` - `#FF793F`
- B (purple / yellow):
  - `color.rgb(114, 48, 131)` - `#723083`
  - `color.rgb(240, 199, 58)` - `#F0C73A`
- C (blue / orange):
  - `color.rgb(56, 104, 174)` - `#3868AE`
  - `color.rgb(215, 107, 52)` - `#D76B34`

## Pine Script / `plot()`

`style` possible values:

- `plot.style_line` (default)
- `plot.style_stepline`
- `plot.style_stepline_diamond`
- `plot.style_histogram`
- `plot.style_cross`
- `plot.style_area`
- `plot.style_columns`
- `plot.style_circles`
- `plot.style_linebr`
- `plot.style_areabr`
- `plot.style_steplinebr`

`linestyle` possible values (v6 only - v5 `plot()` has no such argument):

- `plot.linestyle_solid` (default)
- `plot.linestyle_dashed`
- `plot.linestyle_dotted`

## Pine Script / `hline()`

`linestyle` possible values:

- `hline.style_solid` (default)
- `hline.style_dashed`
- `hline.style_dotted`

## Misc

VSCode plugin to support syntax highlighting:

- https://marketplace.visualstudio.com/items?itemName=ex-codes.pine-script-syntax-highlighter
  - Source Code: https://github.com/yankikucuk/pine-script-syntax-highlighter
