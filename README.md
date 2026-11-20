# Mac-Style Plasma Widgets

<img width="2560" height="1661" alt="Screenshot_20261119_185832" src="https://github.com/user-attachments/assets/2f548f03-0220-4bd9-afb8-652416fc5f1b" />


Calendar, Weather, and News widgets for KDE Plasma 6, styled after the macOS desktop widgets.

I wanted something closer to the look of Apple's widgets without giving up the flexibility of Plasma. The widgets use dark translucent backgrounds, rounded corners, and a layout that changes with the available space. I mainly build and maintain these for my own desktop, but the source is available for anyone who wants to use or modify them.

## Widgets

**Calendar** — Monthly view with clickable dates, month navigation, a Today shortcut, and event markers. Includes US federal holidays and can read events from a public HTTPS `.ics` calendar URL. Calendar imports are read-only; this isn't two-way calendar sync.

**Weather** — Current temperature, conditions, highs and lows, and a seven-day forecast. Set a city and choose Fahrenheit or Celsius in the widget settings. Weather data comes from Open-Meteo.

**News** — Headlines and images from RSS or Atom feeds. Add your own sources in the settings, refresh the feed, and open stories in your browser. Some publishers block automated feed requests, and images depend on what the feed provides.

## Apple-style sizing

Plasma lets you resize desktop widgets manually, but it doesn't offer the same Small / Medium / Large preset workflow as macOS. I added a separate resize watcher to handle that.

| Preset | Size | Layout |
| --- | --- | --- |
| Small | 180 × 180 | Compact square |
| Medium | 380 × 180 | Wide rectangle |
| Large | 380 × 380 | Larger square |

Right-click a widget, open **Configure**, choose a size preset, and apply it. The watcher detects the change and attempts to update the widget's actual desktop dimensions. You can still use Plasma's normal resize handles for a custom size.

The automatic resizing code targets **KDE Plasma 6.8.3**. It works by updating Plasma's desktop layout configuration and may restart Plasma Shell to apply a change. It's more involved than ordinary QML resizing, so behavior on other Plasma versions isn't guaranteed.

## Installation

Requires KDE Plasma 6 with `kpackagetool6`, Qt 6 / Qt Quick, Python 3, and an internet connection for weather and news. The automatic resize feature is intended for Plasma 6.8.3.

Download the source or release archive, extract it, then run:

```bash
chmod +x install.sh
./install.sh
```

Log out and back in to reload the widgets, then add **Calendar**, **Weather**, and **News** from Plasma's widget browser.

The installer sets up a local Python backend and a size watcher. It supports systemd user services and includes an XDG autostart fallback for non-systemd desktops, including OpenRC sessions.

## Configuration

Right-click a widget and choose **Configure** to set its options:

- Calendar: optional public HTTPS ICS calendar URL
- Weather: city and temperature units
- News: RSS/Atom URLs, one per line
- All three: Small, Medium, or Large size preset

The backend listens only on `127.0.0.1:18763`. It handles feed parsing, weather requests, and calendar data. No API key is needed for Open-Meteo.

## Troubleshooting

Check that the backend is running:

```bash
curl http://127.0.0.1:18763/health
```

For problems with automatic resizing, check:

```bash
tail -n 60 ~/.local/share/mac-plasma-widgets/size-watcher.log
```

If the widgets still show an older layout after updating, log out and back in. For RSS errors, check the feed URL in a browser first; not every publisher exposes a public RSS feed.

## Uninstall

From the extracted project directory:

```bash
./uninstall.sh
```

## Notes

This is an independent project inspired by the appearance and sizing of macOS widgets. It isn't affiliated with Apple or KDE. The widgets are written for Plasma rather than being ports of Apple's widget code.
