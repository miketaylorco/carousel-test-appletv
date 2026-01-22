# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## Project Overview

Static HTML project implementing an Apple TV-style hero carousel. No build system, package manager, or framework—vanilla HTML, CSS, and JavaScript in standalone HTML files.

## Files

- **index.html** - Carousel using `clip-path: inset()` wipe transitions
- **index-fade.html** - Alternative using `opacity` fade transitions

## Development

Open either HTML file directly in a browser. No build or server required.

## Architecture

### CSS Transition System
- Slides use CSS classes (`active`, `exiting`) combined with direction classes (`direction-prev`, `direction-next`) on the carousel container
- Both versions include a parallax image effect via `transform: translateX()`
- Transition timing uses `cubic-bezier(0.12, 1, 0.2, 1)` (ease-out: aggressive start, gradual finish)

### JavaScript Carousel Class
- `goTo(index, direction)` - Main transition method handling setup, animation, and cleanup phases
- `cancelTransition()` - Interrupts mid-transition cleanly when user navigates rapidly
- Transitions disable CSS transitions temporarily during setup phases, force reflows, then re-enable for animation

### Pause System
Two independent pause states that combine:
- `userPaused` - Explicit pause via spacebar or button click
- `hoverPaused` - Automatic pause when hovering over text content (uses hoverintent library)

### Auto-advance Progress
Progress bar system with `elapsedTime` tracking that pauses/resumes correctly. Uses interval-based progress updates at 50ms intervals.

### External Dependency
Uses [hoverintent](https://tristen.ca/hoverintent/) library loaded from CDN for intentional hover detection.

### Key Implementation Details
- Text content elements are separate from slides for independent fade timing
- Image elements use `<picture>` with responsive srcsets
- `index.html` uses proper ARIA attributes (role, aria-label, aria-live); `index-fade.html` lacks these accessibility features
