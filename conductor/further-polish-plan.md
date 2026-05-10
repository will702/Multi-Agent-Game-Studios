# Further Design Polish Plan

## Objective
Enhance the tactile and dramatic feel of Stocklab Online through visual feedback and state transitions.

## Implementation Steps

### 1. Phase Announcement Overlay (`main.tsx` & `styles.css`)
*   Create a `PhaseOverlay` component that displays the phase name in large, bold Fira Code across the screen for 2 seconds whenever `state.phase` changes.
*   Use a high-contrast animation (e.g., sliding from center, fading out).

### 2. Sector Shock Notifications (`main.tsx`)
*   Modify `MarketBoard` to flash sectors that just crashed or split.
*   Use the `item.history` or track price changes to trigger a brief CSS animation.

### 3. Transaction Feedback (`main.tsx`)
*   Add a temporary "visual coin" or "floating text" effect near the Player Console when cash changes (+/- coins).

### 4. Hostile Takeover (Akuisisi) Visuals (`main.tsx`)
*   Highlight the target player in the leaderboard with a red "Under Attack" pulse when an Akuisisi card is being hovered or activated.

## Verification
*   Advance through phases and verify the announcement overlay appears.
*   Trigger a sector crash (via Rumor or Economy) and verify the flash.
