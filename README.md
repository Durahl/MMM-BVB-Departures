# MMM-BVB-Departures

A MagicMirror² module that displays live departures from Swiss public transport stops.

Supports:

- BVB buses
- BVB trams
- BLT trams
- Regional buses
- Any stop available through the Swiss Transport API

## Features

- Amber BVB-style display
- Real-time departures
- Delay information
- Multi-instance support
- Automatic stop-ID lookup
- Configurable stop, line and destination
- Supports buses and trams
- Automatic instance ID normalization
- Optional automatic instance ID generation

---

# Requirements

- MagicMirror²
- Node.js
- Internet connection

---

# Installation

## 1. Copy the Module

Navigate to your MagicMirror modules directory:

```bash
cd ~/MagicMirror/modules
```

Extract the ZIP package:

```bash
unzip MMM-BVB-Departures-v4.zip
```

or copy the module folder manually.

You should end up with:

```text
~/MagicMirror/modules/MMM-BVB-Departures
```

Verify the contents:

```bash
ls ~/MagicMirror/modules/MMM-BVB-Departures
```

Expected files:

```text
MMM-BVB-Departures.js
node_helper.js
MMM-BVB-Departures.css
package.json
README.md
```

---

## 2. Install Dependencies

Change into the module directory:

```bash
cd ~/MagicMirror/modules/MMM-BVB-Departures
```

Install all required dependencies:

```bash
npm install
```

If needed, manually install Axios:

```bash
npm install axios
```

---

## 3. Configure MagicMirror

Open your MagicMirror configuration:

```bash
nano ~/MagicMirror/config/config.js
```

Add the module to your `modules` array:

```javascript
{
    module: "MMM-BVB-Departures",
    position: "top_left",
    config: {
        instanceId: "Erlenmatt Bus 30",
        stopName: "Basel, Erlenmatt",
        line: "30",
        destination: "Bahnhof SBB",
        maxDepartures: 4,
        updateInterval: 60000
    }
}
```

---

## 4. Restart MagicMirror

If using PM2:

```bash
pm2 restart MagicMirror
```

Otherwise:

```bash
cd ~/MagicMirror
npm run start
```

---

## 5. Verify Operation

After startup you should see something similar to:

```text
ERLENMATT

[30] BAHNHOF SBB        3'
[30] BAHNHOF SBB        8'
[30] BAHNHOF SBB     12' +2
[30] BAHNHOF SBB       17'
```

Where:

- `3'` = departure in 3 minutes
- `+2` = service delayed by 2 minutes
- Amber text mimics BVB departure boards

---

# Configuration

## Example: Bus Line

```javascript
{
    module: "MMM-BVB-Departures",
    position: "top_left",
    config: {
        instanceId: "Erlenmatt Bus 30",
        stopName: "Basel, Erlenmatt",
        line: "30",
        destination: "Bahnhof SBB",
        maxDepartures: 4
    }
}
```

---

## Example: Tram Line

```javascript
{
    module: "MMM-BVB-Departures",
    position: "top_right",
    config: {
        instanceId: "Bankverein Tram 8",
        stopName: "Basel, Bankverein",
        line: "8",
        destination: "Neuweilerstrasse",
        maxDepartures: 4
    }
}
```

---

# Configuration Options

| Option | Description | Default |
|----------|-------------|----------|
| instanceId | Unique module instance identifier | auto-generated |
| stopName | Stop name | Basel, Erlenmatt |
| line | Transit line filter | "" |
| destination | Destination filter | "" |
| maxDepartures | Max departures displayed | 4 |
| updateInterval | Refresh interval (ms) | 60000 |

---

# Running Multiple Instances

The module supports multiple independent departure boards.

Each instance should have a unique `instanceId`.

## Example

```javascript
{
    module: "MMM-BVB-Departures",
    position: "top_left",
    config: {
        instanceId: "Erlenmatt Bus 30",
        stopName: "Basel, Erlenmatt",
        line: "30",
        destination: "Bahnhof SBB"
    }
},
{
    module: "MMM-BVB-Departures",
    position: "top_right",
    config: {
        instanceId: "Bankverein Tram 8",
        stopName: "Basel, Bankverein",
        line: "8",
        destination: "Neuweilerstrasse"
    }
},
{
    module: "MMM-BVB-Departures",
    position: "bottom_left",
    config: {
        instanceId: "SBB Tram 10",
        stopName: "Basel, Bahnhof SBB",
        line: "10",
        destination: "Rodersdorf"
    }
}
```

---

# Instance IDs

Instance IDs may contain:

- Spaces
- Hyphens
- Underscores
- Mixed case

Examples:

```javascript
instanceId: "Erlenmatt Bus 30"
```

```javascript
instanceId: "Bankverein Tram 8"
```

```javascript
instanceId: "Bahnhof SBB - Line 10"
```

The module automatically normalizes IDs internally.

Examples:

| Configured ID | Internal ID |
|---------------|-------------|
| Erlenmatt Bus 30 | erlenmatt_bus_30 |
| Bankverein Tram 8 | bankverein_tram_8 |
| Bahnhof SBB - Line 10 | bahnhof_sbb_-_line_10 |

This prevents conflicts caused by:

- Different capitalization
- Additional spaces
- Special characters
- Trailing spaces

---

# Automatic Instance IDs

If no `instanceId` is supplied, one is generated automatically from:

```text
stopName + line + destination
```

Example:

```javascript
{
    stopName: "Basel, Erlenmatt",
    line: "30",
    destination: "Bahnhof SBB"
}
```

becomes:

```text
basel_erlenmatt_30_bahnhof_sbb
```

internally.

---

# Optional Filters

## Specific Line and Destination

```javascript
line: "30",
destination: "Bahnhof SBB"
```

---

## Specific Line Only

```javascript
line: "30",
destination: ""
```

---

## Show All Departures

```javascript
line: "",
destination: ""
```

Displays the next departures regardless of line or destination.

---

# Styling

The module uses a BVB-inspired amber display theme.

You can customize colors and fonts by editing:

```text
MMM-BVB-Departures.css
```

Recommended font:

```css
font-family: "Roboto Mono", "Courier New", monospace;
```

---

# Troubleshooting

## Module shows "Loading departures..."

Check MagicMirror logs:

```bash
pm2 logs MagicMirror
```

Look for:

```text
MMM-BVB-Departures helper started
```

and any error messages.

---

## Reinstall Dependencies

```bash
cd ~/MagicMirror/modules/MMM-BVB-Departures

rm -rf node_modules
npm install
```

---

## Test API Connectivity

```bash
curl "https://transport.opendata.ch/v1/locations?query=Basel,Erlenmatt"
```

If JSON is returned, the API is reachable.

---

## Check Configuration Errors

Make sure commas and brackets in `config.js` are correct.

A missing comma is one of the most common MagicMirror startup issues.

---

# Display Example

```text
ERLENMATT

[30] BAHNHOF SBB        3'
[30] BAHNHOF SBB        8'
[30] BAHNHOF SBB     12' +2
[30] BAHNHOF SBB       17'
```

---

# License

MIT License

Use, modify and distribute freely.
