# Mechanics & Economy Implementation Plan

## Objective
Address the mechanical gaps identified in `game_mechanics.md` to ensure Stocklab's core loop functions as intended. The focus is on implementing manual selection for "Quick Buy" and a full private peek UI for "Info Bursa". We will maintain the current deck counts (56 Action / 40 Economic) as decided.

## Key Files & Context
- `src/game/types.ts`: Define new action payloads and player state properties for the peeks.
- `src/game/rules.ts`: Update the `draft` action reducer to handle the new selection mechanics.
- `src/main.tsx`: Add the required UI to support selecting cards/sectors and displaying the peeked cards.

## Implementation Steps

### 1. Update Game Types (`types.ts`)
*   Add `targetCardIds?: string[]` to the `draft` action in `GameAction`.
*   Add `targetSectors?: SectorName[]` to the `draft` action in `GameAction`.
*   Add `infoBursaPeek?: { sector: SectorName; type: EconomicCardType }[]` to `PlayerState` to store the peeked cards.

### 2. Update Game Rules (`rules.ts`)
*   **Quick Buy**: Modify `activateCard` to read `action.targetCardIds`. Validate that exactly 2 valid cards are selected from the pool, then remove them and add to the player's shares.
*   **Info Bursa**: Modify `activateCard` to read `action.targetSectors`. Validate the selection of exactly 2 sectors. Look at the top card of the selected `state.economicDecks` and store the result in the player's `infoBursaPeek` state.

### 3. Clear Peeks on Round End (`rules.ts`)
*   Update `prepareRound` to clear `infoBursaPeek` from all players so old peeks don't carry over to the next round.

### 4. Basic UI Support for Testing (`main.tsx`)
*   *(Note: Deep UX polish will happen in the UX & Flow phase, but basic inputs are needed now for the mechanics to function).*
*   Add a local state in `main.tsx` to hold `selectedCards` for Quick Buy and `selectedSectors` for Info Bursa.
*   Update the `ActionPool` component to allow toggling selection of cards and sectors before clicking "Activate" on these specific cards.
*   Add a simple display in `PlayerPanel` to show the `infoBursaPeek` contents if they exist for the `currentPlayer`.

## Verification
*   Play through an Action Phase.
*   Test Quick Buy: Select 2 cards, click Activate, verify they are removed from the pool and added to holdings.
*   Test Info Bursa: Select 2 sectors, click Activate, verify the private peek UI appears and displays the correct upcoming economic cards.