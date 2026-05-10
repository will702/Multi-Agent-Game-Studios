# Visual UI & Theme Polish Plan

## Objective
Elevate the Stocklab frontend to strictly adhere to the "Exchange Floor board game" aesthetic defined in `DESIGN.md`. The focus is on tactile material realism, strong console-vs-table separation, sharp mono typography, and snappy state feedback.

## Key Files & Context
- `src/styles.css`: The primary target for visual updates.
- `src/main.tsx`: Minor DOM adjustments if necessary to support new CSS styles (e.g., adding classes for active states).
- `DESIGN.md`: The guiding aesthetic document.

## Implementation Steps

### 1. Material Realism & Lighting
*   **Felt & Walnut Deepening**: Introduce subtle SVG noise or CSS grain to the `--felt` and `--walnut` backgrounds in `styles.css`.
*   **Card Thickness**: Update `.game-card` and `.table-preview` shadows to use layered box-shadows that simulate real physical depth (e.g., `0 2px 4px rgba(...), 0 10px 20px rgba(...)`).
*   **Brass Accents**: Enhance the `--brass` color and add a slight inset shadow or gradient to buttons and active states to make them feel metallic and stamped.

### 2. Console vs. Table Contrast
*   **Product Shell Contrast**: Deepen the background of the app shell (`body` or `.app-shell`) to be a richer, darker charcoal (`--panel-2`) to make the felt table pop.
*   **Panel Borders**: Refine `.panel` borders to use a subtle `--line` color that separates the UI controls from the game pieces without being distracting.

### 3. Typography & Data Hierarchy
*   **Mono Accent Tuning**: Ensure all numbers (NAV, coins, prices, bids) strictly use the `Fira Code` stack.
*   **Visual Weight**: Increase the font-weight of critical data points (like the current phase or winner) and apply the `--brass` color more deliberately to highlight the active phase and current turn.

### 4. Snappy State Motion
*   **Card Lifts**: Refine the `.game-card:hover` transition to be faster (`120ms` instead of `180ms`) and more pronounced (a slight scale-up alongside the translation).
*   **Phase Highlights**: Add a subtle pulse or background transition to the active `.phase-step` in the Demo Flow to draw the eye immediately when the phase advances.
*   **Accessibility**: Ensure all new transitions are wrapped in the existing `@media (prefers-reduced-motion: reduce)` block.

## Verification & Testing
*   **Visual Check**: Run the Vite dev server and visually inspect the table, player console, and banker tape.
*   **Theme Adherence**: Verify that no forbidden styles (glassmorphism, pure black, emoji) were accidentally introduced.
*   **Responsiveness**: Ensure the stronger shadows and borders do not break the mobile layout breakpoints (`max-width: 680px`).