
# Bengal Cat Interactive SVG Animation

This project is a small interactive web animation built using **HTML, SVG, CSS, and JavaScript**.  
It renders a layered Bengal cat illustration where different image layers are animated to simulate natural behavior such as **breathing, eye tracking, blinking, head movement, and idle resting**.

The animation is lightweight, requires **no external libraries**, and runs entirely in the browser.

---

# Features

## Eye Tracking
The cat’s pupils follow the user’s mouse cursor.

- Movement is constrained to a small radius (≈6px) so the eyes stay realistic.
- Movement is smoothly interpolated to avoid robotic snapping.
- Cursor distance influences how far the eyes move.

---

## Head Lean Interaction
The cat’s head reacts subtly to the cursor position.

- The head **leans diagonally depending on where the cursor is located**.

Example behavior:

| Cursor Position | Head Reaction |
|---|---|
| Bottom Left | Head leans slightly down-left |
| Top Left | Head leans slightly down-right |
| Bottom Right | Head leans slightly down-right |
| Top Right | Head leans slightly down-left |

This effect is intentionally subtle to keep the animation feeling natural and not exaggerated.

The head movement includes:
- **slight horizontal drift**
- **small rotational tilt**
- **smooth interpolation for realism**

---

## Blinking
The cat blinks automatically at regular intervals.

Blink behavior:

- Quick close
- Brief hold
- Quick reopen

The blink effect is created by temporarily displaying a **closed-eyes overlay layer** above the open-eye layers.

---

## Breathing Animation
Several parts of the cat subtly animate to simulate breathing.

### Head movement
- Small vertical motion synchronized with breathing.

### Body expansion
- A slightly scaled body layer expands and contracts.

### Nose pulse
- The nose layer scales very slightly during breathing to add life to the face.

All breathing animations are driven by a smooth sinusoidal curve.

---

## Idle Behavior (Sleeping)

If the user does not move the mouse for **30 seconds**, the cat assumes the user is inactive and closes its eyes.

Behavior:

- Eyes remain closed while the user is idle.
- The moment the user moves the mouse again, the cat "wakes up".
- Normal blinking resumes immediately.

Breathing continues during idle so the cat appears to be **resting rather than paused**.

---

# Layer Structure

The animation uses **separate PNG layers** exported from a single source image.

Because all layers share the same original coordinate space, they can be stacked perfectly without manual repositioning.

Rendering order (bottom → top):

```
background
body_breathing
body_static
headpart
nose
openeyes
leftEye
rightEye
closedEyesOverlay
```

The **head group** contains all facial elements so they can move together during breathing and head leaning.

---

# Animation System

The animation loop uses:

```
requestAnimationFrame()
```

Each frame performs the following steps:

1. Compute breathing phase
2. Update head vertical breathing motion
3. Compute head lean target based on cursor position
4. Update body and nose breathing scaling
5. Calculate eye target positions
6. Smoothly interpolate eye movement
7. Smoothly interpolate head lean movement
8. Apply SVG transforms
9. Update blink or idle state

All movement is handled with **SVG transforms**, keeping everything in the same coordinate system.

---

# File Structure

Example structure:

```
project/
│
├─ index.html
│
├─ bengal_cat_background.png
├─ bengal_cat_body_static.png
├─ bengal_cat_body_breathing.png
├─ bengal_cat_headpart.png
├─ bengal_cat_openeyes.png
├─ bengal_cat_lefteye.png
├─ bengal_cat_righteye.png
├─ bengal_cat_nose.png
└─ bengal_cat_closedeyes.png
```

---

# Customization

Animation parameters are centralized in the `config` object:

```javascript
const config = {
  maxEyeOffset: 6,
  headBobY: 3,
  noseScale: 0.05,
  bodyScale: 0.03,
  breathDurationMs: 5200,
  blinkEveryMs: 10000,
  blinkDurationMs: 1000,
  eyeEase: 0.12,
  lookRadius: 180,
  idleCloseDelayMs: 30000,

  maxHeadRotateDeg: 0.7,
  maxHeadShiftX: 2,
  headFollowEase: 0.07,
  headLookRadius: 320,
  headLookRadiusY: 320
};
```

Adjust these values to change the character’s behavior.

| Parameter | Effect |
|---|---|
| `maxEyeOffset` | Maximum eye movement |
| `headBobY` | Strength of head breathing motion |
| `noseScale` | Nose breathing intensity |
| `bodyScale` | Body breathing intensity |
| `blinkEveryMs` | Blink interval |
| `idleCloseDelayMs` | Time before cat sleeps |
| `maxHeadRotateDeg` | Maximum head tilt |
| `maxHeadShiftX` | Horizontal head drift |
| `headFollowEase` | Smoothness of head movement |

---

# Browser Compatibility

Works in all modern browsers supporting:

- SVG
- `requestAnimationFrame`
- Pointer events
- CSS transforms

Tested in:

- Chrome
- Firefox
- Edge
- Safari

---

# Potential Fun Upgrades Later On

Ideas for expanding the interaction:

- Background music or ambience
- Purring sound while active
- Sleeping sound / snoring while idle
- Mouth movement
- Occasional “mjau” sound
- Ear twitch animation
- Multiple cats on the same page
