# Drunk Goalkeeper - Game Design

## Core Concept
You're a goalkeeper. Balls fly at your goal. Reach your hands to block them. Miss too many? You drink.

## The 4-Zone System

The goal is divided into 4 quadrants. Your hand position determines which zone you're covering:

| Zone | How to Cover |
|------|--------------|
| TOP LEFT | Reach up + left (10 o'clock) |
| TOP RIGHT | Reach up + right (2 o'clock) |
| BOTTOM LEFT | Reach out left (8 o'clock) |
| BOTTOM RIGHT | Reach out right (4 o'clock) |

### Detection Logic
- **Wrist tracking**: Uses MediaPipe Pose landmarks 15 (right wrist) and 16 (left wrist)
- **Shoulder reference**: Landmarks 11 and 12 for body center
- **Extension threshold**: Hand must extend ~1x shoulder width to register
- **Arms at sides = CENTER** (no zone covered)

```
REACH UP + LEFT  = TOP LEFT     (10 o'clock)
REACH UP + RIGHT = TOP RIGHT    (2 o'clock)
REACH OUT LEFT   = BOTTOM LEFT  (8 o'clock)
REACH OUT RIGHT  = BOTTOM RIGHT (4 o'clock)
```

## Game Flow

1. **Landing** - Clean title, stadium vibe, start button
2. **Instructions** - Visual zone diagram, stick figure demo
3. **Player Setup** - Enter 2-8 player names
4. **Calibration** - Motion test to confirm all 4 positions work
5. **Hot Seat Round** - Player faces shots until 3 goals conceded
6. **Results** - Drinking penalty applied
7. **Next Player** - Rotate through all players
8. **Final Leaderboard** - Crown the best keeper, roast the worst

## Calibration System

Before each player's turn:
1. Player does motion test (touch all 4 zones)
2. System confirms detection is working
3. Ready indicator shows when locked in
4. Handles different body types, distances from camera

## Shot Mechanics

- Ball appears at striker's feet
- Brief "wind up" animation
- Ball flies toward one of 4 zones (smooth glide with spin)
- Player has reaction window to cover that zone
- Success = save, Fail = goal against

### Speed Progression
- Speed increases **2% per shot** for current player
- Caps at **1.5x speed**
- **Resets per player** - each player starts fresh
- Gradual ramp: 1.5x reached around shot 25 (player would be eliminated by then)

## Chaos Mechanics

Triggers every 2nd shot starting from shot 2.

| Mechanic | Probability | What It Does |
|----------|-------------|--------------|
| REVERSED | 20% | Left/right controls flip. Shaky pulsing indicator stays on screen. |
| BLIND | 15% | No red target box (brief 250ms glimpse for fairness). Pure luck shot. |
| FAKEOUT | 25% | Shows fake zone first, then switches to real target. |
| PRESSURE | 30% | 2 shots almost simultaneously. Ultra-fast fixed timings (80ms ball, 60ms windup, 100ms delay). |
| Nothing | 10% | Normal shot. |

## Drinking Rules

Keep it real. Max penalty = bottoms up ONE drink.

| Result | Penalty |
|--------|---------|
| 0-1 saves | Bottoms up. You're benched. |
| 2-3 saves | 2 sips. Rough day at the office. |
| 4 saves | 1 sip. Almost had it. |
| 5+ saves | Clean sheet. Pick someone to drink. |
| Concede 3 in a row | Take a sip mid-game. Momentum killer. |

## Visual Design

### Landing Page
- Stadium aesthetic, clean
- Big title, clear start button
- No wall of text

### Instructions Screen
- Visual diagram showing 4 zones + hand positions
- Stick figure demonstrations
- Drinking rules table
- Punchy copy

### Game Screen
- Goal frame dominant
- Ball animation from striker to goal (smooth glide, 720deg spin)
- Camera feed (320x180, bottom right)
- Current zone indicator
- Score display
- Goals conceded tracker

## Unique Voice

This game's personality:
- Football/soccer commentary vibes
- Dry humor, not try-hard
- "The wall has crumbled", "Absolute howler", "Cat-like reflexes"
- Keep it punchy, not verbose

## Technical Requirements

- MediaPipe Pose for body tracking
- Landmarks: shoulders (11, 12), wrists (15, 16)
- Camera capture: 1280x720 (wide FOV)
- Camera preview: 320x180 (object-fit: contain)
- Smooth zone transitions (debounce flickering)
- Works in limited space

---

## Status

- [x] Clean landing page (stadium aesthetic)
- [x] Clear instructions with zone diagrams
- [x] Working calibration system (motion test)
- [x] Reliable 4-zone detection (hand positions)
- [x] Ball animation + shot mechanics (smooth glide)
- [x] Speed progression (2% per shot, 1.5x cap, per-player reset)
- [x] 4 chaos mechanics (REVERSED, BLIND, FAKEOUT, PRESSURE)
- [x] Scoring + drinking rules
- [x] Multiplayer rotation
- [ ] Final leaderboard polish

---

*Last updated: Session 4 - Chaos mechanics rebalanced*
