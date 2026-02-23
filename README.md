# Multiplayer Yahtzee (Android)
A solo University project of the game Yahtzee focusing on database management to deliver a multiplayer experience between two devices.
![Welcome Picture](media/welcome.png)



**Platform:** Android  
**Language:** Java  
**Multiplayer:** Firebase Realtime Database (room-based sync) + Firebase Anonymous Auth  
**Repo Type:** Portfolio showcase (curated code snippets + demos)

---

## Overview
A turn-based **multiplayer Yahtzee** app for Android. Players can host a room, share a room code, connect to the same match, and play a full Yahtzee game with synchronized turns, dice state, and scoring updates in real time.

---

## Features
- **Host / Join Room Flow** using a generated room code *(see demo1.gif)*
- **Turn-based Multiplayer Sync**: current player turn, dice state, and score state update for all players
- **Yahtzee Rules + Scoring Engine**: validates scoring categories and calculates points
- **Real-time UI Updates** driven by shared game state (dice, categories, scoreboard)
- **Disconnect-safe architecture** (state stored centrally in Firebase so clients can resync)

---

## Demo
### 1) Hosting & Connecting
Shows creating a room and connecting another player via room code:  
`media/demo1.gif`

![Host & Join Demo](media/demo1.gif)


### 2) Gameplay
Shows turn flow, rolling dice, selecting categories, and score updates:  
`media/demo2.gif`

![Host & Join Demo](media/demo2.gif)

---

## Architecture (High Level)
This project uses a **shared game state** model stored in Firebase. Clients:
1. Join a room (gameId)
2. Subscribe to updates (ValueEventListener)
3. Render UI from the latest `GameState`
4. Submit user actions by updating the shared state (dice roll, scoring choice, pass turn)

This makes multiplayer deterministic and easy to reason about: **the database is the source of truth**, and the UI is derived from state.

---

## Code Highlights

### Multiplayer State Models (Shared State)
- Core multiplayer synchronization achieved by subscribing to the room’s GameState in Firebase using a ValueEventListener. 

![Snippet1](media/snippet1.jpg)

- Whenever any player updates the shared state (dice rolls, scoring choice, or turn changes), onDataChange() receives the latest GameState and calls applyStateToUI() to re-render the UI from the source-of-truth state. This keeps both players consistent without relying on local assumptions.

### Real-time Sync + Turn Handling
- This screenshot shows how a turn ends in multiplayer. After a player confirms a scoring choice, the app resets the dice state for the next turn (rolls left, held flags, UI toggles), updates currentTurnUid to transfer turn ownership to the opponent, and writes the updated GameState back to Firebase with gameRef.setValue(currentState). 

![Snippet2](media/snippet2.jpg)

- Because both clients are listening for state changes, the opponent immediately receives the update and their UI becomes active for their turn.
---


## What I Learned / Engineering Focus
- Designing a multiplayer system around a **single source of truth** game state
- Building **turn-based synchronization** that stays consistent across clients

---

## Notes
This repository is intended as a **portfolio showcase**. Full project code and build are available upon request. Please email me at sothearnithsreng@gmail.com