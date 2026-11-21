# MyForecast

A responsive weather dashboard for current conditions, climate telemetry, five-day forecasts, and location-related weather articles.

Built with React, TypeScript, and Vite, MyForecast combines asynchronous REST API integration with weather-responsive glass visuals, accessible interactions, and an installable PWA experience.

**[Live application](https://task-3-nine-phi.vercel.app/)** · **[GitHub repository](https://github.com/eolusoga-804/task-3)**

## Contents

- [Features](#features)
- [Technology stack](#technology-stack)
- [Data providers](#data-providers)
- [Getting started](#getting-started)
- [Available commands](#available-commands)
- [Project structure](#project-structure)
- [API architecture](#api-architecture)
- [Forecast interpretation](#forecast-interpretation)
- [Caching and offline behavior](#caching-and-offline-behavior)
- [Privacy and security](#privacy-and-security)
- [Accessibility and responsive design](#accessibility-and-responsive-design)
- [Installation](#installation)
- [Validation](#validation)
- [Deployment](#deployment)
- [Troubleshooting](#troubleshooting)
- [Known limitations](#known-limitations)
- [Credits and attribution](#credits-and-attribution)

## Features

### Global location search

- Search cities worldwide with autocomplete.
- Distinguish matching locations using region and country information.
- Navigate suggestions with arrow keys, Enter, and Escape.
- Focus search using Ctrl+K on Windows or Command+K on macOS.
- Clear the search input after selecting a location.
- Cancel outdated requests when the search changes.

### Current weather and climate telemetry

- Current temperature, condition description, and weather icon.
- Feels-like temperature when available.
- Humidity, wind speed, compass direction, and wind bearing.
- Atmospheric pressure, visibility, and cloud coverage.
- Sunrise and sunset.
- Hourly rain or snow amounts when supplied by the provider.
- Local date and time, weather calculation time, and time-of-day label.
- Today's forecast range when matching forecast samples are available.

Missing optional measurements display an appropriate fallback.

### Five-day forecast

- Five forecast card slots generated from structured data.
- Daily temperature ranges derived from available three-hour samples.
- Representative condition descriptions and weather icons.
- Location-aware date grouping.
- Explicit unavailable states when the provider supplies insufficient coverage.
- Independent forecast retry controls.

### Current-location detection

- Contextual browser geolocation permission.
- Rounded coordinates used for weather requests.
- Reverse geocoding to retrieve a city or area name.
- Local article lookup after a place name is resolved.
- Coordinate-based weather remains available if name resolution fails.
- Manual city selection takes priority over a pending location result.

### Favorites

- Save manually searched cities and prevent duplicate identifiers.
- Persist favorites and their order in local storage.
- Select or remove saved cities.
- Reorder through a single grip handle using mouse, touch, pen, or keyboard arrows.
- Display drag destination markers, lift effects, and landing feedback.
- Fall back to session-only changes when storage fails.
- Collapse Favorites on mobile and tablet layouts.

Detected coordinates remain session-only and are not automatically saved as favorites.

### Local weather articles

- Retrieve relevant articles from The Guardian.
- Search a larger candidate pool and display up to eight matching articles.
- Filter headlines for the selected place and weather-related terms.
- Remove duplicate destinations.
- Display source and publication date when available.
- Open article destinations in a new tab.
- Provide loading, error, retry, and empty states independently of weather.

### Weather-responsive interface

- Transparent glass panels with blue-purple accents.
- DynaPuff display typography and DM Sans body text.
- Font Awesome interface and condition icons.
- Sunny, cloudy, rainy, and nighttime backgrounds.
- Different decorative rain densities and speeds.
- Nighttime stars and animated cloud effects.
- Introductory glow orbs before selecting a location.
- Reduced-motion alternatives.

### Progressive Web App

- Web app manifest and installation icons.
- Generated service worker for the application shell.
- Compact installation invitation when supported.
- Standalone-mode detection and locally recorded installation status.
- Application metadata, social preview image, favicon, and robots file.

## Technology stack

| Area | Implementation |
| --- | --- |
| Interface | React |
| Language | TypeScript |
| Development and build | Vite |
| Styling | Tailwind CSS and component-scoped BEM styles |
| HTTP | Fetch API, async/await, AbortController |
| Icons | Font Awesome |
| Fonts | DynaPuff and DM Sans |
| Local persistence | Browser localStorage |
| PWA tooling | vite-plugin-pwa and Workbox |
| Code quality | ESLint and TypeScript |
| Hosting and API runtime | Vercel |
| Version control | Git and GitHub |

## Data providers

| Provider | Purpose | Request location |
| --- | --- | --- |
| Open-Meteo Geocoding | City autocomplete | Browser |
| OpenWeather | Current conditions and forecast | Server |
| OpenWeather Geocoding | Reverse location lookup | Server |
| The Guardian Open Platform | Location-related weather articles | Server |

Weather comes from third-party services. The interface does not represent a direct connection to local sensors or guarantee continuously updated observations.

Provider access, quotas, coverage, and account terms apply independently.

## Getting started

### Requirements

- Node.js and npm. Development used Node.js 24.
- Git.
- An OpenWeather API key with access to the required endpoints.
- A Guardian Open Platform API key for local articles.
- Access to a Vercel account/project for the API-enabled development workflow.

### 1. Clone and install

```cmd
git clone https://github.com/eolusoga-804/task-3.git
cd task-3
npm.cmd install
```

### 2. Configure server credentials

Create an ignored `.env` file in the project root:

```dotenv
# Supply credentials locally. Never commit their actual values.
OPENWEATHER_API_KEY=
GUARDIAN_API_KEY=
```

Enter your credentials after the equals signs.

These values belong on the server. Do not rename them with a `VITE_` prefix: Vite exposes those variables in browser bundles.

The Guardian key is required for article retrieval. A missing article configuration should not prevent weather from working.

### 3. Start API-enabled development

```cmd
npx.cmd vercel@latest dev
```

Complete any project-linking prompts and open the localhost address printed by the CLI.

Use the port reported by the terminal rather than assuming a fixed port.

The API-enabled workflow is required for `/api/*` routes. Running Vite alone does not provide the Vercel server functions.

### Frontend-only development

```cmd
npm.cmd run dev
```

This starts the Vite development server for interface work. Weather, reverse location, and article requests require their server endpoints.

## Available commands

| Command | Purpose |
| --- | --- |
| `npm.cmd install` | Install dependencies |
| `npm.cmd run dev` | Start Vite |
| `npx.cmd vercel@latest dev` | Run the interface with Vercel API routes |
| `npm.cmd run lint` | Run ESLint |
| `npm.cmd run build` | Run TypeScript project checks and create the production build |
| `npm.cmd run preview` | Preview built frontend assets |

The frontend preview server does not replace the Vercel API runtime.

The repository currently has no dedicated automated test script. Use the functional checks below alongside lint and build.

## Project structure

```text
myforecast/
├── api/
│   ├── current-weather.ts
│   ├── forecast.ts
│   ├── local-articles.ts
│   └── reverse-location.ts
├── server/
│   └── openWeather.ts
├── public/
│   ├── favicon.svg
│   ├── icon-192.png
│   ├── icon-512.png
│   ├── apple-touch-icon.png
│   ├── site.webmanifest
│   ├── social-card.png
│   ├── robots.txt
│   └── startup.css
├── src/
│   ├── components/
│   ├── hooks/
│   ├── services/
│   ├── styles/
│   ├── types/
│   ├── utils/
│   ├── App.tsx
│   ├── App.css
│   └── main.tsx
├── index.html
├── package.json
└── vite.config.ts
```

Components own their section markup and styles. Services handle requests and normalization. Shared types and utilities keep provider data separate from presentation.

## API architecture

```text
City search
  → Open-Meteo Geocoding
  → Selected location

Current conditions / forecast / reverse lookup / articles
  → Same-origin /api endpoint
  → Provider request using server credentials
  → Validated response
  → Client service and normalized data
  → React interface
```

### Application endpoints

| Endpoint | Parameters | Purpose |
| --- | --- | --- |
| `/api/current-weather` | `lat`, `lon` | Retrieve current conditions |
| `/api/forecast` | `lat`, `lon` | Retrieve forecast samples |
| `/api/reverse-location` | `lat`, `lon` | Resolve coordinates to place labels |
| `/api/local-articles` | `city` | Retrieve filtered article metadata |

Server routes validate inputs and use fixed provider destinations. Requests include timeout handling, and error responses avoid exposing credentials or upstream request URLs.

Client services use asynchronous fetch requests, cancellation, response validation, and understandable failure messages.

Optional article failures do not prevent current weather or forecast sections from rendering.

## Forecast interpretation

Forecast cards use the earliest available five calendar-date groups from the provider response.

The first card may represent today when samples remain for today; otherwise, it starts with the next available date. The interface displays the starting date.

Daily low and high values are calculated from available three-hour readings. They are not guaranteed full-day minimum and maximum observations.

The current-weather summary reuses the matching forecast date for today's range without making an additional forecast request.

## Caching and offline behavior

| Data | Cache behavior |
| --- | --- |
| Current weather and forecast | In-memory cache for up to 60 seconds, bounded to 32 entries |
| Local articles | In-memory cache for up to five minutes, bounded to 20 cities |
| Application shell | Service-worker precaching of built assets |
| Provider API responses | Not stored by the service worker |

Memory caches clear on reload. Article errors are not cached.

The installed application may open its cached shell offline after a successful prior load. Fresh weather, location lookup, city suggestions, and articles still require connectivity.

## Privacy and security

- Provider credentials remain in server environment configuration.
- Local secret files are excluded from version control.
- Geolocation requires contextual browser permission.
- Detected coordinates are rounded before application requests.
- Detected coordinates are not automatically persisted.
- Favorites and their ordering are stored locally in the browser.
- Installation status may be recorded locally.
- The application does not require a user account or preferred-name profile.

Server proxies keep credentials out of client bundles. They are not authenticated user endpoints and still consume provider quota when called.

## Accessibility and responsive design

- Semantic section headings and labeled controls.
- Accessible autocomplete with keyboard navigation.
- Visible project-colored focus indicators.
- Status announcements for loading, errors, and reordering.
- Keyboard alternatives to dragging.
- Hidden accordion controls removed from keyboard navigation.
- Reduced-motion handling.
- Responsive layouts beginning at 320px.
- A maximum dashboard container width of 1140px.
- Collapsible Favorites and weather panels below the desktop breakpoint.
- Minimum-width forecast and telemetry columns.
- Equal-height telemetry cards with centered content.
- Full-width footer with bottom alignment on short pages.

## Installation

Use the custom invitation when the browser reports installation eligibility, or use the browser's installation menu where supported.

On supported Apple devices, the invitation provides manual Add to Home Screen guidance.

Dismissing the invitation applies to the current page load. A detected installation is recorded locally, and the invitation is hidden in standalone mode.

Installation detection varies by browser and profile. Clearing storage removes the local record; uninstalling may leave it behind.

## Validation

Run:

```cmd
npm.cmd run lint
npm.cmd run build
```

Then verify:

- City selection using mouse and keyboard.
- Empty searches, unknown cities, network failures, and retry actions.
- Current conditions, optional telemetry, and five forecast slots.
- Local date, clock, time-of-day label, and weather calculation time.
- Allowed and denied geolocation permission.
- Reverse-location failure without losing coordinate-based weather.
- Favorite saving, removal, persistence, and reordering.
- Mouse, touch, and keyboard reorder interactions.
- Article loading, empty results, failure, retry, and caching.
- Weather and Favorites accordion behavior.
- Reduced-motion rendering and keyboard focus.
- Layout at 320px, common phone widths, tablets, and desktop widths.
- PWA installation and cached-shell behavior in supported browsers.
- Production metadata, icons, manifest, and social preview.

A successful build verifies compilation and bundling. Browser interaction and production checks are separate validation steps.

## Deployment

1. Push the repository to GitHub.
2. Import or connect the project in Vercel.
3. Configure `OPENWEATHER_API_KEY` and `GUARDIAN_API_KEY` for the required Vercel environments.
4. Use the Vite framework configuration, `npm run build`, and `dist` output.
5. Deploy and wait for the production deployment to report Ready.
6. Verify the dashboard and API endpoints on the deployed URL.

Environment changes require a new deployment.

When moving to another production domain, update absolute metadata URLs in `index.html` and the sitemap directive if one is added later.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| Weather service is not configured | Confirm `OPENWEATHER_API_KEY` is available to the server runtime |
| Local articles are not configured | Confirm `GUARDIAN_API_KEY` is configured |
| API routes fail under Vite | Use `vercel dev` for API-enabled development |
| Environment changes do not appear | Restart local development or redeploy Vercel |
| No articles appear | The selected place may have no matching weather headlines |
| Location permission is denied | Continue with manual city search |
| Place name is unavailable | Coordinate-based weather can still load |
| Favorites do not survive reload | Check browser storage permissions |
| Old interface remains after deployment | Close application tabs and installed windows, then reopen |
| Installation invitation is absent | Check browser eligibility, standalone mode, and the local installation record |

## Known limitations

- Weather and article data depend on third-party availability and quotas.
- Reverse geocoding may return a district or nearby locality rather than a broader city.
- Article matching uses place names and weather-related headline terms; ambiguous place names can affect relevance.
- Forecast ranges reflect available samples and may cover partial days.
- The clock-based Morning/Afternoon/Evening/Night label is separate from sunrise-based background styling.
- UV index is not displayed because the current weather integration does not supply it.
- Units currently use Celsius, meters per second, hectopascals, meters, and millimeters.
- The application does not provide historical climate analysis or official severe-weather alerts.
- Offline availability covers the application shell rather than fresh weather data.
- Dynamic weather content is client-rendered; static metadata does not guarantee indexing of every searched city.

## Credits and attribution

Developed by **Emmanuel#804**.

- Weather and reverse geocoding: OpenWeather.
- City autocomplete: Open-Meteo Geocoding.
- Article metadata and destinations: The Guardian Open Platform.
- Interface icons: Font Awesome.
- Fonts: DynaPuff and DM Sans, delivered through Google Fonts.

### Brand artwork

The MyForecast favicon and app icons adapt [Cloud Sun](https://fontawesome.com/icons/cloud-sun?f=classic&s=solid) by Fonticons, Inc., licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).

MyForecast recolors and resizes the icon, adds a gradient background, and exports PNG versions for installation.

Third-party artwork, fonts, content, and services remain subject to their respective licenses and terms.
