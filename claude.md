# Patterning Demand - Recurring Booking Prototypes

## Project Purpose

This project explores different UX patterns for adding recurring booking functionality to the Reflex request form. We're building multiple frontend-only prototypes to test various approaches for how users can request repeating shifts.

## Variant Pages

- **Original** (`request-flex.html`) - Base version, maintained as reference
- **InSequence** (`request-flex-insequence.html`) - Sequential flow approach
- **CalendarView** (`request-flex-calendarview.html`) - Calendar-first approach
- **NewTabFlow** (`request-flex-newtabflow.html`) - Tab-based interface approach
- **Opx** (`request-flex-opx.html`) - Opx variant (same as InSequence for now)

All variants share the same navigation and overall layout. Navigation links are fixed at the bottom of the page (24px from bottom, 304px from left) to easily switch between prototypes.

## Development Notes

- Frontend-only prototypes, no backend integration needed
- Each variant is a separate HTML file
- Maintain consistency in navigation and left sidebar across all variants
- Main content area changes between variants to test different UX patterns
