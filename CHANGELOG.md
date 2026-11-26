# Changelog

Changes to the Stockholm transport departure screen, newest first.

## 2026-11-26

- The footer now shows only disruption messages about lines on the board, or about a stop as a whole. Before, every message for every stop in range rotated through, including ones about lines that were hidden or not shown. SL sometimes links a message to lines it does not mention: the current message about commuter trains at night is linked to the green metro lines, so it still shows wherever those run.
- Cancelled departures are now hidden. SL often lists a cancelled trip next to the one that replaces it at the same time, which filled the board with "Inställd" rows. To show them again, turn on "Visa inställda avgångar" ("Show cancelled departures") in the settings panel or add `cx=1` to the link. Screens pick up the change the next time they load the page.
- Departures are now grouped by stop instead of by transport type. Each stop gets its own block, nearest first, with the stop name, the transport types it serves and the walking time in the header. This shows at a glance where to walk to, and removes the stop name and walking time that were repeated on every row. A metro station and a bus stop with the same name share one block.
- Added "Gruppera avgångar per hållplats" ("Group departures by stop") to the settings panel, on by default. Turning it off, or adding `group=0` to the link, brings back the layout grouped by transport type. Screens that open a link without `group=0` switch to the new layout the next time they load the page.
- "cont." on a section that continues in the next column now stays visible when the section name is too long for the column.
- Updated the README screenshot to show the new layout.
- "Följ min position" now stays on after a reload on iPhone. Safari reports location access as undecided after every reload, which greyed out the setting even while access was still allowed. The page now remembers that access was given on the device and asks for the position directly: Safari answers without a dialog while it still allows access, and shows its permission dialog again once it has forgotten. Declining the dialog turns following off.

## 2026-11-25

- Sections are no longer split across two columns when they can all be kept whole. Before, the board preferred short columns, so a three-type location such as metro, train and bus could get bus split over two columns and an empty half column under the metro. Now each type gets its own column whenever it fits.
- The column arrangement is no longer fixed while departures are still loading. Stops are fetched in batches over the first minute, and an arrangement made from the first batch could split a section across two columns and leave the rest of a column empty. A tidier arrangement now replaces an untidy one after 30 seconds instead of five minutes.
- On wide screens, sections now stay in their columns when departures come and go. The board only rearranges when a departure would otherwise stay hidden for over a minute, or when a tidier arrangement has been possible for five minutes. A transport type that comes back, such as boats at a pier with hourly service, returns to the column it had before.
- Automatic text size now waits three minutes, instead of 20 seconds, before growing, since a larger size can change the number of columns.
- Added "Följ min position" ("Follow my location") to the settings panel. When on, the board moves to the device's current position each time the page is opened or shown again, without reloading the page. It turns on with "Använd min position", turns off when a place is chosen, is greyed out until the browser allows location access, and is never included in shared links.
- Added a footer to the settings panel with the author's name and a link to the source code on GitHub, in Swedish and English.
- Added a screenshot of the board to the README.
- First version of the departure screen: a single static page showing SL metro, commuter train, tram, bus and boat departures near a chosen location, with settings stored in the URL fragment.
