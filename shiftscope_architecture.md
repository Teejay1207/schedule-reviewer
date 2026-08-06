# ShiftScope: Architecture & Technical Overview

## Executive Summary

ShiftScope is a serverless, single-file HTML web application designed to parse weekly schedule images using AI vision models and render them into an interactive, mobile-friendly interface. Built without complex build steps or heavy frameworks, it leverages vanilla JavaScript, modern CSS features, and the Gemini API to deliver a lightweight but powerful utility.

## Architecture

The application is contained entirely within a single `index.html` file. This "App style" HTML structure is ideal for lightweight utilities, simple deployments (like GitHub Pages), and easy distribution. 

### Core Technologies
*   **HTML5/DOM**: Semantic structure and hidden containers for background rendering.
*   **CSS Custom Properties (Variables)**: Drives the design system, theming, and responsive layout.
*   **Vanilla JavaScript**: Handles state management, DOM manipulation, and asynchronous API calls without the overhead of React or Vue.
*   **External Libraries**: Minimal footprint, utilizing only `dom-to-image` via CDN for canvas-based image exporting.

## UI and Design System

The application relies heavily on a custom CSS design system using native variables.

*   **Fluid Typography**: Uses `clamp()` functions to smoothly scale text sizes based on the viewport, eliminating the need for extensive media queries.
*   **CSS Grid & Flexbox**: Defines the overarching layout (sidebar vs. main content) and the internal component structures (metric cards, shift timelines).
*   **Theming**: Uses `[data-theme="light"]` and `[data-theme="dark"]` attributes on the root HTML tag. Toggling themes simply swaps the CSS variables, instantly re-rendering colors across the app without JavaScript overhead.
*   **Role-Based Accents**: Specific roles (Manager, Delivery Expert, CSR) are mapped to unique color hexes, applied dynamically to shift cards via inline CSS variables (`--accent`).

## AI Vision Integration

The core parsing engine relies on the Gemini Vision API to convert unstructured image data into structured JSON.

*   **Prompt Engineering**: The system prompt instructs the AI to enforce specific rules (e.g., mapping "DE" to "Delivery Expert", calculating hours, noting Open/Close flags) and demands a strict, minified JSON response.
*   **Fallback Strategy**: The `callGemini` function iterates through a prioritized array of models (e.g., `gemini-3.1-flash-lite`, `gemini-3.5-flash`). If one model fails or throws a 503 error, it seamlessly attempts the next in the chain.
*   **Rate Limit Handling**: Specific logic captures `429 Too Many Requests` status codes. Instead of silently retrying and burning through quota, it halts execution and exposes a clear warning block in the UI.

## State Management and Rendering

State is managed via a single global JavaScript object, creating a pseudo-reactive loop similar to modern frameworks but much simpler.

*   **Global State**: A `state` object tracks the search query, parsed shifts, processing status, and current theme.
*   **Data Aggregation**: Before rendering, the app iterates over the shift data to calculate total hours per employee and group shifts by day.
*   **DOM Updates**: A central `render()` function is called whenever state changes (e.g., a new search query or successful API response). It clears and repopulates the DOM elements (tables, shift timelines, metric counts) based on the filtered and aggregated data.

## Image Generation (Export feature)

To allow users to text or share the summarized schedule, the app dynamically generates a PNG image.

1.  **Hidden Rendering Container**: A div (`#exportContainer`) is positioned off-screen (`left: -9999px`). 
2.  **HTML Injection**: During the `render()` cycle, clean, print-friendly HTML containing the employee hour totals is injected into this hidden container.
3.  **Canvas Conversion**: When the "Save as Image" button is clicked, the `dom-to-image` library reads the DOM of the hidden container, calculates computed styles, converts the nodes into an SVG, and then renders it to a Canvas.
4.  **File Download**: The Canvas is converted to a base64 PNG data URL and programmatically downloaded to the user's device.
