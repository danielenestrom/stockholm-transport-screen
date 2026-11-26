# Stockholm transport departure screen

A full-screen departure board for SL metro, commuter trains, trams, buses and boats near a chosen location in Stockholm. It is a single static page meant for a wall-mounted screen, and works on any static host.

![The board showing metro, bus and boat departures near Slussen](docs/screenshot.png)

This is an independent project. It is not made or endorsed by SL (Storstockholms Lokaltrafik) or Trafiklab.

## Files

- `stockholm-transport/index.html` is the page: markup, styles and script.
- `stockholm-transport/fonts/` holds the D-DIN font files and their licence. Publish them together with the page.
- `.claude/launch.json` is a preview configuration for Claude Code.

## Run locally

```bash
python3 -m http.server 8765 --directory stockholm-transport
```

Then open `http://localhost:8765/`.

## Data sources

The page is based on information retrieved from [Trafiklab.se](https://www.trafiklab.se/). Both APIs need no key and allow calls from a browser. Their use is covered by the [terms for SL APIs on Trafiklab](https://www.trafiklab.se/api/other-apis/sl/licence/), which require this attribution and ask users not to make excessive requests. The page footer shows the attribution; keep it if you change the page.

- SL Transport API, `https://transport.integration.sl.se/v1`: `/sites` and `/stop-points` for the stop list (cached in the browser for 24 hours), `/sites/{id}/departures` for departures.
- SL journey planner, `https://journeyplanner.integration.sl.se/v2/stop-finder`: address and stop search in the settings panel.

SL sometimes answers with an empty departure list for a stop that does have departures. The page only believes an empty answer when it comes twice in a row, and keeps the previous times until then. SL returns HTTP 429 when requests arrive too fast. The page asks for at most 12 stops per refresh, two at a time, retries a 429 twice and keeps the previous times for a stop that fails. The refresh interval is 30 to 60 seconds depending on the number of stops; the countdown is recalculated every second in between.

## Link parameters

Settings are stored in the URL fragment, so a link opens the board with the same location and settings. Example:

```
https://example.com/stockholm-transport/#lat=59.3196&lon=18.0722&name=Slussen&r=600&hide=boat&lang=en
```

| Parameter | Meaning | Default |
| --- | --- | --- |
| `lat`, `lon` | Location in decimal degrees | T-Centralen |
| `name` | Label shown in the header | The coordinates |
| `addr` | Address to look up instead of `lat` and `lon` | |
| `r` | Radius in metres | 500 |
| `hide` | Transport types to hide: `metro`, `train`, `tram`, `bus`, `boat` | none |
| `show` | Alternative to `hide`: the only types to show | all |
| `hidedir` | Directions to hide, as line and SL direction code `1` or `2`, for example `76:1,17:2` | none |
| `lang` | `sv` or `en` | `sv` |
| `win` | Show departures within this many minutes | 60 |
| `walk` | `0` also shows departures that leave before you can walk to the stop | on |
| `size` | Text size: `a` (automatic), `s`, `m`, `l` or `x` | `a` |
| `led` | `0` switches from the dot-matrix typeface to a plain one | on |
| `msg` | `0` hides disruption messages | on |
| `group` | `0` groups departures by transport type instead of by stop | on |
| `cx` | `1` also shows cancelled departures | off |
| `plat` | `0` hides platform, stand and berth | on |
| `split` | `0` keeps both directions of metro and commuter trains in one block | on |

Keyboard: `S` opens settings, `F` toggles full screen.

## Follow my location

On a phone, tablet or laptop, "Följ min position" in the settings panel moves the board to the device's current position each time the page is opened or shown again (unlocking the phone, switching back to the tab or window). The page itself is not reloaded; only departures are fetched again.

- "Använd min position" turns following on. Choosing an address or stop, or opening a link to another place, turns it off.
- The setting is greyed out until location access has been given once on the device, or while the browser blocks it. Once given, the setting stays on across reloads, even though Safari reports access as undecided after every reload.
- With following on, the page asks for the position when it is opened or shown again. If the browser still allows it, the position comes back with no dialog. If the browser has forgotten the permission (Safari does after about a day when the site is set to "Ask"), the browser shows its permission dialog again. Declining it turns following off, so the page does not keep asking.
- Moves under 75 metres are ignored, and the position is read at most every 30 seconds.
- The setting is saved on the device and is not part of the link, so a shared link or a wall screen never follows anyone's location.

## Line colours

Colours are set in the `COL` table in `index.html`. Metro lines use their line colour, commuter trains pink, buses red or blue (blåbuss), and each tram and light rail line its own colour. Boats use a single teal that is not an SL colour.

## Layout rules

- Departures are grouped by stop, nearest first. Each stop's header shows its name, the transport types it serves and the walking time, so the rows only show line, destination, platform or stand, and times. Within a stop, departures are listed by transport type and then by time. Stops that SL lists separately under the same name, such as a metro station and a bus stop, share one block.
- Metro and commuter trains get one block per direction, with the direction under the stop name, for example "mot Hässelby strand". The direction is SL's end station for each line in that direction, so a block can list several. Each metro line colour (green, red, blue) is split on its own, and the stop's other departures follow in one block. Turn this off with "Dela upp tunnelbana och pendeltåg per riktning" (`split=0`).
- Under "Riktningar" in the settings panel, each line nearby is listed with the destinations it has in each direction. Unticking a direction hides it from the board and adds it to `hidedir` in the link. Direction codes mean different things on different lines, so use the panel rather than guessing them. A hidden direction stays in the list, so it can be turned back on, even when the line has no departures at the moment.
- With "Gruppera avgångar per hållplats" turned off (`group=0`), departures are grouped by transport type instead, and each row shows its stop and walking time.
- Automatic text size picks the largest size at which every row fits, and never goes below a size readable from a distance. If rows still do not fit, those from the farthest stops are dropped first. The text shrinks at once when rows no longer fit, but only grows after the larger size has fitted for three minutes.
- Each destination is shown at full size when it fits its column. Otherwise it steps down to 84 % and then 71 % of full size, only as far as needed. A name a few pixels too wide gets slightly tighter letters, and a name that does not fit even then scrolls slowly to its end and back, with a pause at each end. Scrolling is off when the device asks for reduced motion.
- A section is only split across two columns when there is no arrangement that keeps every section whole, even if that makes the columns taller.
- Sections keep their columns between updates, once every stop in range has been asked for departures. The board is rearranged when a departure would stay hidden for more than a minute, when a tidier arrangement (no empty column, fewer split sections) has been possible for 30 seconds, or when the screen size, location or settings change.
- Walking time is the straight-line distance to the stop at 75 metres per minute. Departures that leave sooner than that are hidden.
- A line and destination served by several stops in range is shown for the nearest stop only.
- The footer shows only disruption messages that SL links to a line on the board, or to a stop as a whole. A message about a line that is hidden, outside the time window or not served nearby is left out.
- Cancelled departures are hidden, and a line with only cancelled departures in the time window is left out. With "Visa inställda avgångar" turned on (`cx=1`), a row whose next departures are cancelled shows "Inställd" and the cancelled times.

## Privacy

- The page has no server of its own and no analytics. Settings and the stop list are saved in the browser's local storage.
- The browser sends stop ids and place searches to SL. The chosen coordinates are not sent; distances are calculated in the browser. The same applies to the device position when following is on.
- The plain typeface, [D-DIN](https://www.datto.com/fonts/d-din), is hosted with the page. The dot-matrix typeface, [Doto](https://github.com/oliverlalan/Doto), is loaded from Google Fonts, so Google sees the visitor's IP address. To avoid that, host the Doto font file yourself and change the stylesheet link in `index.html`.

## Deploy

Copy the `stockholm-transport` folder to any static web host. There is no build step.

## Licence

The code is released under the [MIT licence](LICENSE). The licence covers the code only. SL data is covered by the Trafiklab terms linked under Data sources, and the Doto and D-DIN typefaces by the SIL Open Font License 1.1 (D-DIN's copy is in `stockholm-transport/fonts/`).
