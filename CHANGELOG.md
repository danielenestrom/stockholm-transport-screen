# Changelog

Changes to the Stockholm transport departure screen, newest first.

## 2026-11-25

- Added "Följ min position" ("Follow my location") to the settings panel. When on, the board moves to the device's current position each time the page is opened or shown again, without reloading the page. It turns on with "Använd min position", turns off when a place is chosen, is greyed out until the browser allows location access, and is never included in shared links.
- Added a footer to the settings panel with the author's name and a link to the source code on GitHub, in Swedish and English.
- Added a screenshot of the board to the README.
- First version of the departure screen: a single static page showing SL metro, commuter train, tram, bus and boat departures near a chosen location, with settings stored in the URL fragment.
