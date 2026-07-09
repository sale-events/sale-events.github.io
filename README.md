# SaleEvents Technical Specifications

This open-source repository is dedicated strictly to maintaining public technical configurations, PWA (Progressive Web App) architecture blueprints, and cross-subdomain interoperability specifications for multi-regional architectures.

## Production Environment
The live production environment is accessible at the primary domain:

**[SaleEvents](https://sale-events.com/)**

---

## Multi-Subdomain PWA Seamless Navigation Blueprint

To guarantee a native app experience across the global multi-regional infrastructure, this layout implements the modern Chromium `scope_extensions` protocol to eliminate fallback URL address bars during cross-origin context switches.

### 1. Bidirectional Association Specification (`/.well-known/web-app-origin-association`)

All participating trustworthy nodes within the matrix MUST expose the verification asset at the exact location: `https://<domain>/.well-known/web-app-origin-association`.

#### Critical Implementation Pitfalls (Chromium Constraints)
* **Flat Dictionary Enforcement:** Legacy nested array syntax (`"web_apps": [...]`) is completely deprecated. Modern Chromium and Edge rendering engines will silently ignore nested objects. A flat JSON pattern mapping the absolute origin to its valid scope context must be used.
* **Server Protocol Strictness:** The endpoint MUST return a `Content-Type: application/json` header. Furthermore, the route **must not invoke any HTTP redirections** (e.g., 301/302 status codes, trailing slash appending, or unencrypted HTTP-to-HTTPS lag). Any redirection will instantly invalidate verification.

#### Compliant Flat JSON Validation Blueprint
```json
{
  "https://sale-events.com/": { "scope": "/" },
  "https://au.sale-events.com/": { "scope": "/" },
  "https://ca.sale-events.com/": { "scope": "/" },
  "https://hk.sale-events.com/": { "scope": "/" },
  "https://ie.sale-events.com/": { "scope": "/" },
  "https://jp.sale-events.com/": { "scope": "/" },
  "https://mt.sale-events.com/": { "scope": "/" },
  "https://nz.sale-events.com/": { "scope": "/" },
  "https://ph.sale-events.com/": { "scope": "/" },
  "https://sg.sale-events.com/": { "scope": "/" },
  "https://tw.sale-events.com/": { "scope": "/" },
  "https://uk.sale-events.com/": { "scope": "/" },
  "https://us.sale-events.com/": { "scope": "/" }
}
```

---

### 2. Manifest Architecture Definition (`scope_extensions`)

#### Crucial Configuration Constraints
* **Self-Domain Exclusion:** The `scope_extensions` array MUST NOT include the origin of the current serving domain. The legal scope boundary of the current host is inherently declared by the root `scope` key. Declaring itself will lead to parsing anomalies.
* **The N - 1 Cardinality Rule:** Given a total network matrix size of N domains, any specific node's manifest array size MUST precisely equal N - 1. For this 13-origin topology, exactly 12 external origins must be injected into the array.

---

### 3. Debugging & Verification Cache Lifecycle

Due to aggressive browser engine caching for origin associations, standard reloading (F5 / Ctrl+F5) will fail to reflect structural backend mutations. Developers must enforce the following sequence for validation:

1. Close all active standalone PWA client containers.
2. Completely uninstall the PWA from the OS/Browser profile environment (check 'Clear browsing data' if prompted).
3. Return to a standard browser tab, navigate to the targeted domain, and initialize DevTools (`F12`).
4. Head to `Application` -> `Storage` -> Trigger **`Clear site data`**.
5. Reload the page, execute a clean PWA installation, and perform cross-origin redirection testing.

---
*Disclaimer: This repository hosts technical configurations and open-source schemas only. SaleEvents operates purely as an independent informational layout and does not sell any products, bookings, or services, nor is it affiliated with, authorized, or endorsed by any third-party brands or agencies. All product and company names are trademarks™ of their respective holders.*
