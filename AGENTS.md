# AGENTS.md - Nissan Leaf Homey App

## Project Overview

This is a Homey app that integrates with Nissan Leaf vehicles (pre-May 2019). The app communicates with Nissan's servers via the `leaf-connect` library to provide battery status, charging control, and climate control features.

## Build, Lint, and Test Commands

### Installation
```bash
npm install
```

### Linting
```bash
npm run lint        # Run ESLint
npm run lint-fix    # Run ESLint with auto-fix
```

### Local Development
```bash
homey app run      # Run app locally on Homey
homey app build    # Build the app for production
```

### Testing
No test framework is currently configured. Do not add tests unless explicitly requested.

## Code Style Guidelines

### General

- **Language**: JavaScript (not TypeScript). Type definitions (`@types/homey`) are provided for IDE support only.
- **Module System**: CommonJS (`require()` / `module.exports`)
- **Strict Mode**: Always use `'use strict';` at the top of every file
- **File Encoding**: UTF-8

### Imports

```javascript
// Built-in Homey modules
const { Driver } = require('homey');
const { Device } = require('homey');

// External dependencies
const leafConnect = require('leaf-connect');
```

### Formatting

- 2 spaces for indentation (no tabs)
- No trailing whitespace
- Line length: follow ESLint defaults (typically 80-120 chars)
- Use semicolons
- Use single quotes for strings

### Naming Conventions

- **Classes**: PascalCase (e.g., `LeafDriver`, `LeafDevice`)
- **Variables/Methods**: camelCase (e.g., `username`, `updateCapabilities`)
- **Constants**: UPPER_SNAKE_CASE when truly constant
- **Files**: kebab-case (e.g., `driver.js`, `device.js`)

### Classes

```javascript
'use strict';

const { Driver } = require('homey');

class LeafDriver extends Driver {

  async onInit() {
    this.log('LeafDriver has been initialized');
  }

}

module.exports = LeafDriver;
```

### Async/Await

- Always use `async`/`await` for asynchronous operations
- Never use raw promises without await
- Handle errors with try/catch blocks

```javascript
async onInit() {
  try {
    const client = await leafConnect({ username, password, regionCode });
    const status = await client.cachedStatus();
    return status;
  } catch (error) {
    this.error(error);
    throw error;
  }
}
```

### Error Handling

- Always wrap async operations in try/catch
- Use `this.error()` to log errors
- Throw meaningful errors with descriptive messages
- Never swallow errors silently without logging

```javascript
session.setHandler('validate', async (data) => {
  if (!data.username) throw Error('Enter username');
  if (!data.password) throw Error('Enter password');
});
```

### Logging

- Use `this.log()` for general information
- Use `this.error()` for errors
- Include context in log messages

```javascript
this.log('LeafDevice updateCapabilities cachedStatus:', status);
```

### Capabilities and Settings

- Use Homey capability system for device features
- Store credentials in device settings (not in code)
- Always use `.catch(this.error)` when calling capability setters

```javascript
this.setCapabilityValue('measure_battery', Number(value)).catch(this.error);
```

### Homey-Specific Patterns

- Drivers handle device discovery and pairing
- Devices handle actual communication and state
- Use `this.homey.setInterval()` for polling (not native setInterval)
- Use `registerCapabilityListener()` for actionable capabilities

### ESLint Configuration

The project uses `eslint-config-athom` (extends "athom" config). Run `npm run lint` before committing.

### File Structure

```
app.js                    # Main app entry point
drivers/
  leaf/
    driver.js             # Driver (pairing, discovery)
    device.js             # Device (state, communication)
    pair/
      start.html          # Pairing UI
    driver.compose.json   # Driver metadata
    driver.settings.compose.json
    assets/               # Icons and images
locales/
  en.json                 # English translations
  no.json                 # Norwegian translations
app.json                  # App manifest
```

### What NOT to Do

- Do not commit secrets, API keys, or credentials
- Do not modify the `.homeybuild/` directory (generated)
- Do not add TypeScript compilation unless required
- Do not change module system to ES modules
- Do not remove `'use strict';` from files
