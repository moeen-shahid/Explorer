# Explorer — Map + Weather + Traffic + Transit

A lightweight, static web app built for **OT Hacks 2026** that brings together everything you'd want to know about a place on one map: live weather and air quality, nearby points of interest, real-time traffic flow and incidents, turn-by-turn routing, and live transit arrivals — all running client-side with no backend required.

## Features

- 🗺️ **Interactive map** — [Leaflet](https://leafletjs.com/) + OpenStreetMap tiles, no API key needed for the base map
- 🔍 **Place search** — Geocode any address or place name via OpenStreetMap/Nominatim
- 📍 **Use My Location** — One-click geolocation with graceful fallback if denied
- ☁️ **Current weather** — Live conditions for the selected location via Weatherbit
- 🌧️ **Weather radar overlay** — Global precipitation radar tiles via RainViewer (no API key)
- 🌫️ **Air quality** — US AQI and pollutant breakdown via Open-Meteo (no API key)
- 📌 **Nearby points of interest** — Search restaurants, gas stations, cafes, hotels, parking, pharmacies, supermarkets, and ATMs via TomTom Search, plotted on the map
- 🚦 **Live traffic** — TomTom traffic flow tiles plus a real-time incident feed (accidents, congestion, road closures) with a color-coded legend
- 🧭 **Route planner** — Set start/end points by typing an address or clicking the map, choose a travel mode (car, truck, taxi, bus, van, motorcycle, bicycle, walking), and get distance, duration, ETA, and turn-by-turn directions via TomTom Routing
- 🚌 **Live transit arrivals** — Real-time predictions for TTC (Toronto) via the NextBus/UmoIQ feed, and TransLink (Vancouver) via the RTTI API

The app is designed to degrade gracefully: features that need an API key simply stay quiet (with a setup prompt) if a key isn't configured, while everything that doesn't need a key — the base map, radar, and air quality — works immediately.

## Tech stack

| Component | Service / Library | API key required? |
|---|---|---|
| Base map & markers | [Leaflet](https://leafletjs.com/) + OpenStreetMap tiles | No |
| Geocoding (search) | OpenStreetMap Nominatim | No |
| Current weather | [Weatherbit](https://www.weatherbit.io/) | Yes |
| Weather radar overlay | [RainViewer](https://www.rainviewer.com/) | No |
| Air quality | [Open-Meteo Air Quality API](https://open-meteo.com/en/docs/air-quality-api) | No |
| POI search | [TomTom Search API](https://developer.tomtom.com/search-api) | Yes |
| Traffic flow & incidents | [TomTom Traffic API](https://developer.tomtom.com/traffic-api) | Yes |
| Routing & directions | [TomTom Routing API](https://developer.tomtom.com/routing-api) | Yes |
| TTC arrivals (Toronto) | UmoIQ / NextBus public feed | No |
| TransLink arrivals (Vancouver) | [TransLink RTTI API](https://developer.translink.ca/) | Yes |

## Quick start

### 1. Add your API keys

Copy the example config and fill in your keys:

```bash
cp config.example.js config.js
```

Edit `config.js`:

```js
window.APP_CONFIG = {
  WEATHERBIT_API_KEY: "YOUR_WEATHERBIT_API_KEY",
  TOMTOM_API_KEY: "YOUR_TOMTOM_API_KEY",
  TRANSLINK_API_KEY: "YOUR_TRANSLINK_API_KEY", // optional — only needed for Vancouver transit
};
```

> `config.js` is git-ignored. The app still runs without any keys — the map, weather radar, air quality, and TTC arrivals all work key-free — but weather, POI search, traffic, routing, and TransLink arrivals will be disabled until keys are added.

### 2. Run a local static server

A local server is recommended so geolocation works correctly. Pick one:

```bash
# Node, no install
npx serve . -p 5173

# Node (http-server)
npx http-server -p 5173

# Python 3
python -m http.server 5173
```

Or in VS Code, install the **Live Server** extension and open `index.html` with it.

Then open **http://localhost:5173**.

## Usage

- **Search** a place using the top input (powered by OpenStreetMap/Nominatim) — press Enter to recenter the map and refresh weather/AQI.
- Click **📍** to use your current location (requires HTTPS or localhost).
- Pick a **POI category** and click **Find Nearby** to plot TomTom search results on the map and list them in the sidebar.
- Click **🚦 Traffic** to toggle live traffic flow tiles and the incidents panel.
- Click **☁️ Weather Overlay** to toggle the RainViewer radar layer on the map.
- Click **🗺️ Route** to open the route planner — set a start and end point (by typing an address or clicking the map), choose a travel mode, and view distance, duration, ETA, and step-by-step directions.
- In the **Transit** panel, choose **TTC** or **TransLink**, enter a stop ID, and click **Get Arrivals** for live predictions.

## Getting API keys (free tiers)

- **Weatherbit** (Current Weather API): https://www.weatherbit.io/account/create
- **TomTom** (Search, Traffic, Routing): https://developer.tomtom.com/
- **TransLink** (RTTI API, optional, Vancouver only): https://developer.translink.ca/

OpenStreetMap tiles, Nominatim geocoding, RainViewer radar, Open-Meteo air quality, and TTC arrivals are all free and require no key.

**Important:** Restrict your API keys in each provider's console (e.g. HTTP referrer restrictions for browser/JS keys), since this is a static, client-side app and keys are visible in network requests. Keep `config.js` private — it's already excluded via `.gitignore`.

## Project structure

```
index.html
styles.css
config.example.js
config.js          # created by you, ignored by git
js/
  main.js
```

## Notes

- This is a fully static site — no backend, build step, or bundler required.
- If the map fails to load, check your network connection or CDN availability for Leaflet/OSM tiles.
- Weatherbit and TomTom free tiers may rate-limit; the UI will show a generic failure message if a request fails.
- POI search radius/limit and result count can be tuned in `js/main.js`.
- Traffic incident severity is color-coded (green → free flow, through red → blocked).

## Contributors

- [Moeen Shahid](https://github.com/moeen-shahid)
- Ryan Mampilly

## License

This project is licensed under the [MIT License](LICENSE).
