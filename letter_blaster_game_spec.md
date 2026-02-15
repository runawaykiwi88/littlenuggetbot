# Letter Blaster Game - Development Specification

## Project Overview
A simple letter recognition game for a 4-5 year old. Letters fall slowly from the top of the screen, and the child presses the matching key on a physical keyboard. When correct, the character "blasts" the letter with a random fun effect (fireball, ice, fireworks, hearts, or laser). After 5 correct answers, there's a brief celebration.

**Platform:** Web page (HTML/CSS/JavaScript)
**Target User:** 4-5 year old child, sitting with parent
**Key Constraint:** Child is new to keyboards, needs slow pace and simple mechanics

---

## Visual Style

### Overall Aesthetic
- **8-bit pixel art style** for characters and effects
- **Modern, calming gradient background** (appealing to parents)
- **Colorful but not overwhelming** - vibrant game elements on calm background
- **No flashing or distracting UI elements** outside of gameplay

### Character Design
- Use the provided pixel art character (friendly orange blob with robot arms)
- Character should have subtle idle animation (gentle bounce every 2-3 seconds)
- Character positioned at bottom-center of screen

---

## Color Palette

### Background Gradient (Calm, Parent-Friendly)
```css
background: linear-gradient(180deg, #667eea 0%, #764ba2 100%);
```
- Top: `#667eea` (Soft Purple-Blue)
- Bottom: `#764ba2` (Deep Purple)

**Alternative palettes if needed:**
- Peachy: `#ffecd2` → `#fcb69f`
- Mint: `#a8edea` → `#fed6e3`

### Game Elements
- **Letters:** `#FFD700` (Golden Yellow)
- **Letter Outline/Shadow:** `#FF6B35` (Orange) - 4px offset
- **Score/Text:** `#FFFFFF` (White) with subtle text shadow
- **Progress indicators:** White outlined circles

### Blast Effect Colors
- **Fireball:** `#FF4500` (Red-Orange) with `#FFD700` (Yellow) center
- **Ice:** `#00CED1` (Turquoise) with `#E0F7FA` (Icy White)
- **Fireworks:** Multi-color (`#FF69B4`, `#FFD700`, `#00CED1`, `#9370DB`)
- **Hearts:** `#FF69B4` (Hot Pink) with `#FFB6C1` (Light Pink)
- **Laser:** `#00FF00` (Bright Green) with `#FFFFFF` (White) core

---

## Layout & Sizing

### Screen Layout
```
┌─────────────────────────────────────┐
│  Score: 12  ◐ Progress (◯◯◉◉◉)      │  ← Top bar (60px height)
├─────────────────────────────────────┤
│                                     │
│            [Letter A]               │  ← Letter starts here (y: 80px)
│               ↓                     │     Descends slowly
│         (moving down)               │     Speed: 60px/second
│               ↓                     │
│                                     │
│                                     │
│         👾 [Character]              │  ← Character centered bottom
│                                     │     (y: viewport height - 180px)
└─────────────────────────────────────┘
```

### Element Specifications
- **Letter font size:** 200px (massive for visibility)
- **Letter descent speed:** 60 pixels/second (very slow - critical for 4-year-old)
- **Character size:** 200px × 200px
- **Character position:** Bottom center, 180px from bottom edge
- **Top score bar:** 60px tall, semi-transparent dark overlay (`rgba(0,0,0,0.3)`)
- **Margins:** 40px padding on score bar elements

### Typography
- **Font:** `Press Start 2P` (Google Fonts - authentic 8-bit feel)
- **Score bar text:** 20px
- **Letters:** 200px, bold
- **Celebration text:** 48px

---

## Game Mechanics

### Core Gameplay Loop
1. Random uppercase letter appears at top of screen (y: 80px)
2. Letter descends at constant speed (60px/second, linear - no easing)
3. Child presses matching key on keyboard
4. If correct:
   - Random blast effect plays
   - Letter is destroyed
   - Score increments
   - Progress counter increments
   - New letter appears immediately (after 200ms pause)
5. If incorrect key pressed:
   - Nothing happens (ignore wrong keys)
6. If letter reaches bottom of screen:
   - Letter disappears (no penalty)
   - New letter spawns

### Progress & Celebration
- Track progress toward celebration (5 correct = celebration)
- Display progress as 5 circles: ◯◯◯◯◯
- Filled circles (◉) show progress: e.g., ◯◯◉◉◉ means 3/5
- After 5 correct answers:
  - Character bounces (500ms animation)
  - Confetti falls from top (2 seconds)
  - "Great Job!" text appears (1 second display)
  - Progress resets to 0
  - Game continues

### Letter Selection
- Random from A-Z (uppercase only)
- Ensure variety (don't repeat same letter consecutively if possible)

---

## Animation Specifications

### Letter Movement
- Spawn position: `top: 80px`, centered horizontally
- Movement: Constant linear descent at 60px/second
- If letter Y position > viewport height: despawn and spawn new letter

### Blast Animations (5 Types - Selected Randomly on Correct Key)

#### 1. Fireball (200ms total duration)
```
0-50ms:   Small orange circle appears at character position
50-100ms: Ball grows while traveling toward letter position
100-150ms: Impact! Explosion with 8-10 orange/yellow particles spreading
150-200ms: Letter fades out, particles dissipate
```

#### 2. Ice (250ms total duration)
```
0-80ms:   Blue ice shard shoots from character to letter
80-150ms: Letter gets blue overlay (freezing effect)
150-200ms: Letter shatters into 8 ice pieces
200-250ms: Pieces fall downward and fade out
```

#### 3. Fireworks (300ms total duration)
```
0-100ms:  Sparkle trail shoots upward from character to letter
100-150ms: Burst into 12-15 colorful particles at letter position
150-300ms: Particles spread outward radially and fade
Letter disappears in the sparkle effect
```

#### 4. Hearts (250ms total duration)
```
0-100ms:  5-7 pink hearts float from character upward to letter
100-180ms: Hearts circle around the letter (orbital animation)
180-250ms: Letter and hearts fade together with slight scale-up
```

#### 5. Laser (150ms total duration)
```
0-50ms:   Bright green beam shoots from character to letter (instant)
50-100ms: Beam pulses twice (brightness variation)
100-150ms: Letter vaporizes with pixelated dissolve effect
```

### Character Idle Animation
- Subtle bounce every 2-3 seconds
- Movement: Slight vertical offset (±5-10px)
- Duration: 300ms ease-in-out
- Should loop continuously while game is running

### Celebration Animation
- Character jumps (scale + Y offset): 500ms
- Confetti particles (30-40 pieces) fall from top over 2 seconds
- "Great Job!" text fades in (200ms), holds (1s), fades out (200ms)
- Confetti colors: Use blast effect palette for variety

---

## Technical Implementation Notes

### HTML Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Letter Blaster</title>
    <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div id="game-container">
        <div id="score-bar">
            <span id="score">Score: 0</span>
            <span id="progress">◯◯◯◯◯</span>
        </div>
        
        <div id="letter">A</div>
        
        <div id="character">
            <img src="character.png" alt="Friendly Character">
        </div>
        
        <canvas id="effects-canvas"></canvas>
        
        <div id="celebration"></div>
    </div>
    
    <script src="game.js"></script>
</body>
</html>
```

### Key JavaScript Logic Points

**Game State Variables:**
```javascript
let currentLetter = '';
let score = 0;
let progressCount = 0;
const letters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('');
const blastTypes = ['fireball', 'ice', 'fireworks', 'hearts', 'laser'];
let letterY = 80;
const descentSpeed = 60; // pixels per second
```

**Core Functions Needed:**
- `spawnNewLetter()` - Select random letter, reset position
- `updateLetterPosition(deltaTime)` - Move letter down based on time elapsed
- `handleKeyPress(event)` - Check if pressed key matches current letter
- `playBlastAnimation(type)` - Trigger appropriate blast effect
- `celebrate()` - Show celebration sequence
- `updateUI()` - Update score and progress display
- `gameLoop(timestamp)` - Main game loop using requestAnimationFrame

**Keyboard Listener:**
```javascript
document.addEventListener('keydown', (e) => {
    const pressedKey = e.key.toUpperCase();
    
    if (pressedKey === currentLetter) {
        handleCorrectPress();
    }
    // Ignore incorrect keys - do nothing
});
```

**Game Loop Pattern:**
```javascript
let lastTime = 0;
function gameLoop(currentTime) {
    const deltaTime = currentTime - lastTime;
    lastTime = currentTime;
    
    updateLetterPosition(deltaTime);
    
    requestAnimationFrame(gameLoop);
}

// Start
spawnNewLetter();
requestAnimationFrame(gameLoop);
```

### Canvas for Blast Effects
Use HTML5 Canvas for particle effects (fireworks, hearts, explosions, etc.)
- Canvas should overlay the entire game container
- Set canvas to `pointer-events: none` so it doesn't block clicks
- Clear and redraw each frame for active animations

### Progress Display
Update progress circles based on `progressCount`:
```javascript
function updateProgress() {
    const filled = '◉'.repeat(progressCount);
    const empty = '◯'.repeat(5 - progressCount);
    document.getElementById('progress').textContent = filled + empty;
}
```

---

## File Structure
```
letter-blaster/
├── index.html
├── styles.css
├── game.js
├── character.png (provided by user)
└── README.md (optional)
```

---

## Development Phases

### Phase 1: Basic Functionality (Start Here)
- [ ] Set up HTML structure with gradient background
- [ ] Display single letter that falls at correct speed
- [ ] Keyboard input detection (correct key only)
- [ ] Letter respawns on correct key press
- [ ] Score counter works
- [ ] Letter respawns if it goes off-screen

### Phase 2: Visual Polish
- [ ] Add character sprite at bottom
- [ ] Implement progress tracker (5 circles)
- [ ] Style score bar properly
- [ ] Add 8-bit font throughout

### Phase 3: One Blast Effect
- [ ] Implement ONE blast animation (suggest starting with Fireball - simplest)
- [ ] Test timing and visual appeal
- [ ] Ensure letter disappears correctly after blast

### Phase 4: All Blast Effects
- [ ] Add remaining 4 blast types
- [ ] Random selection on correct key press
- [ ] Verify all animations complete properly

### Phase 5: Celebration & Polish
- [ ] Add celebration sequence after 5 correct
- [ ] Character idle animation
- [ ] Confetti effect
- [ ] Final visual polish

### Phase 6: Testing & Refinement
- [ ] Test with actual 4-year-old user
- [ ] Adjust letter descent speed if needed
- [ ] Verify all letters work correctly
- [ ] Cross-browser testing

---

## Critical Requirements (Don't Skip These)

1. **Letter must fall VERY slowly** - 60px/second is critical for a first-time keyboard user
2. **Wrong keys do nothing** - no negative feedback, only positive
3. **Immediate new letter spawn** - keep momentum going (200ms pause is fine)
4. **Large, clear letters** - 200px font size minimum
5. **Random blast effects** - keeps it fresh and exciting
6. **No scary elements** - all effects should be friendly/playful
7. **Progress visible** - child can see they're working toward celebration

---

## Testing Checklist

- [ ] Letter appears at top and falls smoothly
- [ ] Correct key press triggers blast effect
- [ ] Incorrect key press does nothing (no error)
- [ ] Score increments properly
- [ ] Progress circles update correctly
- [ ] After 5 correct, celebration plays
- [ ] Game continues after celebration
- [ ] All 5 blast types work and look good
- [ ] Character has idle animation
- [ ] Letter respawns if it goes off screen
- [ ] Works on target browser (Chrome/Firefox/Safari)
- [ ] Character sprite displays correctly
- [ ] Responsive to different screen sizes

---

## Assets Needed

### Provided by User
- `character.png` - The orange blob pixel art character (attached to project)

### To Be Created/Found
- Font: Press Start 2P (from Google Fonts - free)
- Particle effects: Generated via Canvas API
- Confetti: Generated via Canvas API
- All other graphics: CSS/Canvas-based

---

## Nice-to-Have (Future Enhancements)

These are NOT required for initial version but could be added later:

- Sound effects for each blast type
- Background music (toggleable)
- Difficulty levels (faster letter descent)
- Lowercase letters mode
- Letter pronunciation audio
- Parent dashboard showing which letters child struggles with
- Mobile/touch support (optional since primarily keyboard-focused)
- High score tracking

---

## Notes for Developer

- Start simple: Get basic letter falling and keyboard detection working first
- Test frequently with the actual user (4-year-old)
- The descent speed is CRITICAL - err on the side of slower
- Blast animations should be quick but satisfying
- Keep the codebase simple and readable for future modifications
- Consider using requestAnimationFrame for smooth animations
- Use Canvas for particle effects rather than DOM manipulation (better performance)

---

## Questions to Consider During Development

1. Should letter respawn immediately or have a brief pause? (Spec says 200ms - test this)
2. What happens if child holds down a key? (Suggest: ignore repeat events)
3. Should there be a "start screen" or just begin immediately?
4. Do we need a way to pause/reset the game?

**Current Answers:**
- 200ms pause between letters seems right
- Ignore key repeats (use keydown event, check for e.repeat)
- Start immediately (child is sitting with parent who can refresh if needed)
- No pause/reset needed for MVP

---

## Success Criteria

The game is successful if:
1. A 4-year-old can understand what to do within 30 seconds (with parent help)
2. Child can find and press keys at their own pace without feeling rushed
3. Positive feedback (blast effects) are delightful and encouraging
4. Game runs smoothly without lag or bugs
5. Parent finds it visually appealing (not annoying to sit through)
6. Child wants to play again after finishing

---

## Final Note

This is a learning tool disguised as a game. The goal is to make keyboard interaction fun and letters recognizable. Keep it simple, keep it positive, and keep it slow enough for a new keyboard user to succeed. The parent will be there to help, so the game doesn't need to be fully self-explanatory through text/audio alone.

Good luck with the build! 🚀
