# Changelog

Changes to the Stockholm transport departure screen, newest first.

## 2026-11-26

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
