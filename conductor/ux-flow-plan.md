# UX & Flow Implementation Plan

## Objective
Streamline the game experience and improve readability. We will implement automated phase countdowns and a prominent active player overlay to ensure players always know when and what they need to do.

## Key Files & Context
- `src/main.tsx`: Implement the countdown timer logic and the Active Player Overlay component.
- `src/styles.css`: Add styles for the overlay and the animated timer progress bar.

## Implementation Steps

### 1. Automated Phase Countdown (`main.tsx`)
*   Add `timeLeft` local state to `App`.
*   Implement a `useEffect` that counts down every second when `phaseDuration(state.phase) > 0`.
*   When `timeLeft` reaches 0:
    *   In **Bidding**: Automatically lock a minimum bid (1 coin) for any player who hasn't bid.
    *   In **Action**: Skip the player's turn or auto-draft (keep) a random card? (Wait, the rules say "-1 coin penalty" and move to next). I will implement the -1 coin penalty and move to the next turn.
    *   In **Selling**: Auto-pass the player's turn with a -1 coin penalty.
*   Reset `timeLeft` whenever the phase or active seat changes.

### 2. Phase Timer Visuals (`main.tsx` & `styles.css`)
*   Replace the static timer text in the phase panel with an animated progress bar that shrinks as time runs out.
*   Change the bar color to red when less than 3 seconds remain.

### 3. Active Player Overlay (`main.tsx` & `styles.css`)
*   Create an `ActiveOverlay` component that displays "IT'S YOUR TURN" or "AWAITING ACTION" over the `PlayerPanel`.
*   Only show the interactive version of the overlay to the `currentPlayer` if they are the `activePlayer`.
*   Add a visual "Your Turn" pulsing border to the player's console.

### 4. Dynamic Phase Instructions (`main.tsx`)
*   Update `phaseCopy` to be more instructional based on the specific player's context (e.g., "Select 2 cards for Quick Buy").

## Verification
*   Start a game. Verify the 10s Bidding timer starts automatically.
*   Wait for the timer to expire and verify the -1 coin penalty and phase advancement.
*   Verify the "Active Player" overlay appears correctly when it's a specific player's turn in the Action phase.
*   Ensure the timer resets correctly when a player makes a move.