# Begrateful Context

## Project Objective

Begrateful is an art game about forced gratitude during a period of social and cultural change. The commentary is that the world is going through a lot, but the only emotion the player is allowed to have is gratitude for whatever little they have. They are not allowed to be critical, negative, angry, or sad about the changes around them.

The player performs gratitude by smiling into the camera. Their smile score fills a coin wallet. Each deposited coin is worth `$1000`. When the wallet reaches milestones, the player can spend that earned money on food.

## Current Experience

The current prototype is a single static `index.html` page.

- It asks for camera permission.
- It shows the camera feed inside a centered circular frame.
- It uses MediaPipe Face Landmarker in the browser to estimate whether the player is smiling.
- It reads the `mouthSmileLeft` and `mouthSmileRight` blendshape scores.
- It does not show the smile score directly on screen.
- When the current smile score is more than `80%`, a circular ticker appears on the yellow camera ring with a continuous loop of `I am grateful.`
- Before permission, the camera frame is large and centered. After permission, it stays centered while the player learns to smile. Once the player earns at least three coins while smiling, the camera animates into a fixed top-center bubble around `160px x 160px`. If the player stops smiling, it animates back to the centered position while the alarm/blinker is active.
- Camera docking animation should stay visually anchored to the center of the camera circle, without side-to-side drift while zooming.
- When camera detection is running and the current smile score is at or below the `80%` threshold, the screen enters an emergency alarm state with flashing red overlay and large `Be Grateful` text.
- It fills the bottom money wallet when the current smile score is more than `80%`.
- It drains the money wallet when the smile score is `80%` or lower, or when no face is detected.
- The wallet spans the full viewport width and is visualized as coins instead of a conventional progress fill.
- The wallet header shows three stats: `Maximum earning`, `Calories consumed`, and `Current balance`.
- The wallet stats should be visually centered and grouped close together rather than spread across the full viewport.
- On mobile, the wallet stats should not stack vertically; they should loop horizontally in a compact single-line marquee row without an obvious snap/reset.
- `Current balance` is the deposited coin count multiplied by `$1000`.
- `Maximum earning` is the highest current balance reached during the session.
- `Calories consumed` is the total calories from purchased/eaten foods during the session.
- While the player is earning progress, a coin animates from the right side of the wallet lane, flipping between `coin1.png` and `coin2.png`.
- Deposited wallet coins stack flush from left to right using `coin3.png`.
- When the smile drops below the earning threshold, coins withdraw from the stack one at a time in the opposite direction at the same animation speed.
- If the smile crosses the threshold while a coin is moving, the in-flight coin animation should cancel and reverse direction immediately. Coin direction should be based on the current/raw threshold state, not a smoothed score.
- The visible wallet value is based on the deposited coin stack, not the internal smile-progress accumulator.
- The blue wallet strip should match the visible deposited coin stack width.
- Wallet stops:
  - `10%`: Banana
  - `25%`: Bread
  - `60%`: Chicken
  - `100%`: Cake
- Reaching a stop opens a dialog that congratulates the player and lets them eat the earned food or keep going.
- Earned-food dialog titles should use natural grammar, such as `You earned a banana`, not `You earned banana`.
- The pre-purchase reward dialog should not include a sentence like `Congratulations, you can now eat...`; the title, nutrition facts, food image, and CTAs are enough.
- Milestone dialogs pause wallet motion. Coin inflow/outflow should stop while the dialog is open and resume only after the dialog closes.
- Milestone dialogs show the corresponding food image asset.
- Milestone dialogs show nutrition facts for the food.
- The food price appears only in the purchase CTA, which includes the food name plus dollar amount, for example `Buy Banana - $12,000`.
- Choosing `Eat` resets wallet progress to `0%`, clears earned milestones, and shows a message like `Yummy, you consumed X calories.`
- In the post-eating `Yummy` dialog state, the remaining CTA should say `So grateful!`.
- In post-eating `Yummy` dialog states, the food image should loop through the matching consumed-frame series when available.
- Banana uses `assets/BananaEaten-1.png` through `assets/BananaEaten-4.png`; Bread, Chicken, and Cake each use `Eaten-1.png` through `Eaten-6.png` for their respective food names.
- After food is purchased/eaten, the matching food icon in the progress/wallet milestone bar should also be replaced by that food's consumed-frame loop.
- Wallet milestone icons must render inside fixed-size boxes so whole/eaten frame swaps do not change the progress bar height.
- The bottom wallet/food milestone area should stay compact and not take excessive vertical space.
- On mobile, the bottom wallet/food milestone area should be especially compact.
- In the pre-purchase reward dialog, the secondary CTA should say `I need to save money`; clicking it closes the dialog and preserves progress.
- After clicking `I need to save money`, money flow should pause rather than immediately draining while the player is not smiling. The alarm can continue. Money flow resumes once the player becomes grateful/smiles above threshold again.
- Food milestones should be earnable again after the wallet drops below that milestone and later reaches it again.
- Progress pauses while the dialog is open.

## Artistic Direction

The look and feel should move toward an arcade game:

- Pixel art
- Primary colors
- Bright, vibrant, loud visual language
- Surrealist imagery and interactions
- Celebration should feel intense when the player earns food or eats food
- The subject matter is dark, but the surface should be colorful and playful

Avoid making the game feel like a neutral productivity app. It should feel like an arcade cabinet for compulsory optimism.

## Core Metaphor

The coin stack is the player's money wallet.

Smiling is labor. Gratitude is performance. Food is purchased with proof of acceptable emotion. Each coin is worth `$1000`.

## Technical Notes

- The app is currently plain HTML, CSS, and JavaScript in `index.html`.
- It runs locally with a static server, for example:

```sh
python3 -m http.server 8000
```

- Open the page at:

```text
http://localhost:8000
```

- Camera access should be tested from `localhost`; opening the file directly may block camera APIs.
- MediaPipe is loaded from jsDelivr:

```text
https://cdn.jsdelivr.net/npm/@mediapipe/tasks-vision@0.10.35/vision_bundle.mjs
```

- Face Landmarker model:

```text
https://storage.googleapis.com/mediapipe-models/face_landmarker/face_landmarker/float16/latest/face_landmarker.task
```

## Change Log Rule For Future Agents

Every agent who changes code in this project must also update this `context.md` file in the same turn.

When making code changes:

- Add a short dated note under `Change Log`.
- Mention what changed.
- Mention any new behavior, tuning values, dependencies, or important assumptions.
- If a feature is partially implemented or untested, say that clearly.

This file is the project memory. Keep it current so anyone who picks up the project can understand what has happened and where the game is headed.

## Change Log

### 2026-05-25

- Created the initial static camera prototype in `index.html`.
- Added a centered circular camera frame.
- Added camera permission flow using `navigator.mediaDevices.getUserMedia`.
- Added MediaPipe Face Landmarker smile detection using mouth smile blendshape scores.
- Added live expression and smile score display.
- Added full-width wallet/progress bar that fills when smile score is more than `80%` and drains otherwise.
- Added wallet stops for Banana, Bread, Chicken, and Cake.
- Added reward dialog behavior for eating earned food or keeping progress.
- Added this `context.md` project memory and established the rule that future agents must update it whenever code changes are made.
- Restyled `index.html` toward the intended arcade-game direction using primary colors, pixel-like hard edges, chunky black outlines, offset shadows, uppercase monospace text, a striped wallet bar, and louder reward dialog styling.
- Replaced wallet stop text labels with food image assets from `assets/`: `Banana.png`, `Bread.png`, `Chicken.png`, and `Cake.png`.
- Removed the visible smile score/expression panel from `index.html` and added a circular gratitude ticker around the camera that appears only when the current smile score is more than `80%`.
- Adjusted the camera composition so the camera is smaller, the yellow ring is thicker, and the ticker text sits directly on the yellow camera ring with larger type.
- Removed the top `BEGRATEFUL` label above the camera and changed the circular ticker copy to `I am grateful.`
- Replaced the visual wallet/progress bar fill with coin animation assets from `assets/`: `coin1.png` and `coin2.png` for the flying animated coin, and `coin3.png` for deposited coins stacked from left to right.
- Tuned the coin wallet so deposited coins sit directly next to each other, incoming coins travel more slowly, and milestone/progress display is based on deposited coin stack value.
- Tightened deposited coin rendering with slight overlap, made the blue wallet strip width follow the deposited coin stack, and changed the total coin count to be calculated from the wallet viewport width so the coin stack and progress value stay aligned.
- Added mirrored coin withdrawal: when smile score falls below the `80%` threshold, deposited coins leave the stack one at a time toward the right using the same `1400ms` travel speed as deposits, and in-flight coin animations cancel on direction changes.
- Added a gratitude alarm state: once detection is active, dropping below the `80%` smile threshold flashes a translucent red screen overlay with large red/white `Be Grateful` warning text.
- Removed delayed coin-direction changes by driving coin deposit/withdrawal from the current smile threshold state. Dropping below `80%` now cancels incoming coins and starts withdrawal immediately, while rising above `80%` cancels withdrawal and starts deposits immediately.
- Renamed the wallet display from `Smile progress` to `Money` and changed the visible value from percent to dollars. Each deposited coin is worth `$1000`.
- Replaced the single wallet money readout with three stats: maximum earning, calories consumed, and current balance. Current balance still uses `$1000` per deposited coin; maximum earning tracks the session peak, and calories consumed accumulates after purchases.
- Centered the three wallet stats in a constrained-width row so the labels and values sit closer together.
- Fixed the mobile wallet stats marquee so duplicated stat sets sit in one continuous horizontal strip instead of appearing as two stacked rows. Mobile stat labels and values now render on the same line to reduce height.
- Paused coin wallet animation while reward dialogs are open and added the matching food image to reward and consumed-food dialog states.
- Removed the separate food price badge from reward dialogs, added food-specific nutrition facts, and kept the price only inside the purchase CTA. Prices are calculated from the milestone stop percentage and current wallet coin capacity, with each coin worth `$1000`.
- Center-aligned reward dialog content/actions and changed the post-eating dialog CTA from `Keep going` to `So grateful!`.
- Changed the pre-purchase secondary dialog CTA from `Keep going` to `I need to save money` and ensured reward dialog actions are centered.
- Added a banana consumed animation for the `Yummy` dialog by looping through `BananaEaten-1.png`, `BananaEaten-2.png`, `BananaEaten-3.png`, and `BananaEaten-4.png`; the loop stops when the dialog closes.
- Added consumed-frame loops for Bread, Chicken, and Cake Yummy dialogs using their respective `BreadEaten-1.png` through `BreadEaten-6.png`, `ChickenEaten-1.png` through `ChickenEaten-6.png`, and `CakeEaten-1.png` through `CakeEaten-6.png` assets.
- Updated wallet milestone food icons so after a food is purchased, its progress-bar image changes from the whole food asset to the matching consumed-frame loop.
- Fixed wallet milestone icon sizing so consumed-frame loops do not make the progress bar jump vertically.
- Reduced the vertical height of the bottom wallet area by shrinking dock padding, coin lane height, flying/deposited coin sizes, and food milestone icon boxes.
- Added mobile-specific wallet compaction with shorter dock padding, smaller coin lane, smaller coin sprites, smaller food milestone icons, and reduced mobile body bottom padding.
- Changed the camera docking behavior: it no longer moves immediately after permission. It now animates to an approximately `160px x 160px` top-center bubble only after the player earns at least three coins while smiling, and animates back to center as soon as the player stops smiling.
- Adjusted camera docking CSS/animation math to anchor movement from the center of the camera circle instead of top-left positioning, reducing sideways drift during zoom in/out.
- Updated reward dialog title grammar with per-food articles, e.g. `You earned a banana` and `You earned some bread`.
- Removed the pre-purchase reward dialog sentence `Congratulations, you can now eat...` while keeping the consumed-calorie message in the Yummy state.
- Changed save-money behavior so dismissing a reward dialog pauses wallet inflow/outflow until the player smiles above the `80%` threshold again. Also re-arms reached food milestones once the wallet drops below them, allowing the same food dialog to appear again after losing and re-earning enough money.
