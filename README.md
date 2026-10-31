# Timing Side-Channel Visualizer

An interactive, single-file HTML/JavaScript teaching tool that visualizes how an early-exit
string comparison leaks secret data one character at a time through response timing, and how a
constant-time comparison closes that leak. Built for SE/CprE 4210 Learning Sprint #3 (Problem 5).

## What it does

The page simulates comparing a guess against a secret value using two different comparison
strategies, selectable with a toggle:

- **Vulnerable: early-exit comparison** &mdash; simulates code like
  `for (i = 0; i < len; i++) { if (guess[i] != secret[i]) return false; }`, which stops the moment
  it hits a mismatch. The bar chart shows, per character position, how many "work units" the
  comparison performed before bailing out.
- **Fixed: constant-time comparison** &mdash; simulates a comparison that always walks the full
  length regardless of where (or whether) a mismatch occurs, the standard defense used by
  functions like `MessageDigest.isEqual` or `hmac.compare_digest`.

Typing a guess into the input box updates the bar chart and status message live, showing exactly
how much "time" the vulnerable comparison spent, and highlighting (in a different color) the
prefix of characters it has confirmed correct, visualizing the information leak directly.

## Features

- **Live text input** &mdash; type any guess and watch the simulated timing bars update per character position
- **Vulnerable vs. constant-time toggle** &mdash; directly compare the leaky and fixed implementations side by side
- **"Auto-attack" button** &mdash; runs a simulated timing attack that recovers the secret one character at a
  time by keeping whichever next character increases the simulated comparison time, demonstrating
  how a real timing side-channel attack turns an exponential guessing problem into a linear one
- **Color-coded leak highlighting** &mdash; confirmed-correct character positions are visually distinguished in vulnerable mode

## What it deliberately does not do

This tool does not measure real wall-clock timing, make any network requests, or compare against
any real credential or secret. All "timing" is a simulated, deterministic work-unit count computed
locally in the browser purely for visualization. It demonstrates the *mechanism* of a timing
side-channel, not a working attack against a live system.

## Running it

No build step or dependencies, it's a single static HTML file.

```bash
# Clone the repo, then just open it in a browser:
open index.html        # macOS
xdg-open index.html    # Linux
```

Or serve it locally:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Files

- `index.html`: the full app (markup, styles, and logic in one file)
- `reflection.md`: reflection on the build process and what it taught me about timing side-channels

## Built with

HTML, CSS, and vanilla JavaScript. No frameworks or external libraries, so it runs anywhere
with zero setup.
