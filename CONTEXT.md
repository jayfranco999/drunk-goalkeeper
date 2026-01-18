# Drunk Goalkeeper - Session Context

## How to Resume This Project

When starting a new Claude session for this project, paste this at the start:

```
I'm continuing work on Drunk Goalkeeper, a body-tracking drinking game.

Key files to read:
- /Users/jayfrancis/claude-test/drunk-goalkeeper/GAME_DESIGN.md (mechanics + design)
- /Users/jayfrancis/claude-test/drunk-goalkeeper/CONTEXT.md (this file)
- /Users/jayfrancis/claude-test/drunk-goalkeeper/index.html (current implementation)
- /Users/jayfrancis/claude-test/GAME_DESIGN_PRINCIPLES.md (overall design principles)

[Then describe what you want to work on next]
```

---

## Project Summary

**What is this?**: Body-tracking goalkeeper game. Reach your hand to 4 positions (10, 2, 8, 4 o'clock) to cover goal zones. Block shots or drink.

**Tech Stack**: Single HTML file, MediaPipe Pose, vanilla JS, CSS animations

**Live URL**: https://jayfranco999.github.io/drunk-goalkeeper/

---

## Current State

**Status**: WORKING - Core mechanics functional, chaos modes polished

**What's done**:
1. Clean landing page (stadium aesthetic)
2. Clear instructions with zone diagram
3. Hand-based detection (reach up+left, up+right, out left, out right)
4. 4 chaos mechanics working and rebalanced
5. Speed progression (2% per shot, 1.5x cap, per-player reset)
6. Calibration with motion test
7. Smooth ball animation (glide with spin)

---

## The 4-Zone System

```
| Zone         | Hand Position                    |
|--------------|----------------------------------|
| TOP LEFT     | Reach up + left (10 o'clock)     |
| TOP RIGHT    | Reach up + right (2 o'clock)     |
| BOTTOM LEFT  | Reach out left (8 o'clock)       |
| BOTTOM RIGHT | Reach out right (4 o'clock)      |
```

Detection uses:
- Shoulders (landmarks 11, 12) for body center
- Wrists (landmarks 15, 16) for hand position
- Must extend hand ~1x shoulder width to register
- Arms at sides = CENTER (no zone)

---

## Chaos Mechanics

Triggers every 2nd shot (starting from shot 2).

| Mechanic | Probability | What It Does |
|----------|-------------|--------------|
| REVERSED | 20% | Left/right controls flip. Pulsing indicator stays on screen. |
| BLIND | 15% | No red target box (250ms glimpse for fairness). Pure luck shot. |
| FAKEOUT | 25% | Shows fake zone first, then switches to real target. |
| PRESSURE | 30% | 2 shots almost simultaneously. Ultra-fast fixed timings. |
| Nothing | 10% | Normal shot. |

---

## Speed Progression

- Speed increases **2% per shot** for current player
- Caps at **1.5x speed**
- **Resets per player** - each player starts fresh at 1x
- Affects: shot timing, ball flight, delays between shots
- Gradual ramp: 1.5x by shot 25 (player would be eliminated by then anyway)

---

## Design Decisions

- **Hand positions** (not lean/crouch) - More intuitive, works in limited space
- **Calibration per player** - Motion test confirms all 4 positions work
- **Max drinking penalty** = bottoms up ONE drink
- **Unique voice** - football commentary vibes, dry roasts
- **Hot seat multiplayer** - one player at a time, rotate through
- **REVERSED indicator** - 24px pulsing glow so player knows controls are flipped
- **BLIND mode fairness** - 250ms brief glimpse of target zone
- **Ball animation** - Smooth glide with 720deg spin (not teleport)

---

## Session Log

### Session 1
- Initial fixes to detection logic and landing
- User feedback: left/right reversed, animations missing, landing still bad

### Session 2
- Complete visual rebuild (FIFA penalty style)
- CSS goalkeeper + striker characters
- Ball animation from striker to goal

### Session 3
- **Detection rewritten** - Changed from lean/crouch to hand positions (8/4 o'clock more horizontal)
- **4 chaos mechanics implemented**:
  - REVERSED with persistent shaky indicator
  - BLIND (no red target, ~15% chance)
  - FAKEOUT (fake zone then switch)
  - PRESSURE (2 shots almost simultaneous)
- **Speed progression added** - 5% faster per shot, caps at 2x

### Session 4
- **Speed rebalanced**:
  - Changed from 5% to **2% per shot**
  - Changed cap from 2x to **1.5x**
  - **Resets per player** - fair start for everyone
- **PRESSURE mode kept brutal**:
  - Second shot uses fixed ultra-fast timings (80ms ball, 60ms windup)
  - Only 100ms delay between shots
  - Not affected by speed scaling - always maximum chaos
- **Hosted on GitHub Pages**

### Session 5 (Current)
- **Camera capture**: 1280x720 (wide FOV)
- **Camera preview**: 320x180 with object-fit: contain
- **Ball animation fixed**: Reset inline transition, smooth 0.7s glide with 720deg spin
- **BLIND mode fairness**: 250ms brief glimpse of target
- **REVERSED indicator enhanced**: 24px with pulsing glow animation
- **Chaos rebalanced**:
  - Triggers every 2nd shot (was 3rd)
  - FAKEOUT 25%, PRESSURE 30% (were lower)
- **Pushed to GitHub**

---

## Key Code Locations

- **State object**: Line ~1073
- **Detection logic** (`detectPose`): Line ~1187
- **Speed function** (`getSpeed`): Line ~1091
- **Chaos mechanics** (`maybeChaos`): Line ~1593
- **Shot logic** (`takeShot`): Line ~1493
- **Game loop** (`gameLoop`): Line ~1457
- **Ball animation CSS**: Line ~589

---

*Last updated: Session 5 - Chaos rebalanced, ball animation fixed, REVERSED indicator enhanced*
