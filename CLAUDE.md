# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static HTML project implementing an Apple TV-style hero carousel component. There is no build system, package manager, or framework—just vanilla HTML, CSS, and JavaScript in standalone HTML files.

## Files

- **index.html** - Main carousel using clip-path wipe transitions between slides
- **index-fade.html** - Alternative version using fade/opacity transitions

## Development

Open either HTML file directly in a browser. No build or server required.

## Architecture

The carousel is implemented as a single-file component with embedded CSS and JavaScript:

### CSS Transition System
- Slides use CSS classes (`active`, `exiting`) combined with direction classes (`direction-prev`, `direction-next`) on the carousel container
- `index.html` uses `clip-path: inset()` for wipe reveal effects
- `index-fade.html` uses `opacity` for crossfade effects
- Both versions include a parallax image movement effect via `transform: translateX()`

### JavaScript Carousel Class
- `goTo(index, direction)` - Main transition method that handles setup, animation, and cleanup phases
- `cancelTransition()` - Interrupts mid-transition cleanly when user navigates rapidly
- Transitions disable CSS transitions temporarily during setup phases, force reflows, then re-enable for animation
- Auto-advance uses a progress bar system with `elapsedTime` tracking that pauses/resumes correctly

### Pause System
Two independent pause states that combine:
- `userPaused` - Explicit pause via spacebar or button click
- `hoverPaused` - Automatic pause when hovering over text content (uses hoverintent library for intentional hover detection)

### External Dependency
Uses [hoverintent](https://tristen.ca/hoverintent/) library loaded from CDN for hover detection.
