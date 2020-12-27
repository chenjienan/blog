---
title: 独立游戏开发 - Unity UI for Health Bar
p: GameDev
date: 2020-12-26 17:43:45
category: gamedev
---

# Unity UI for Health Bar

## Create the visuals in PhotoShop

- Rounded Rectangle


## Import Assets in Unity

- Mesh Type - Full Rect: allow sprite editing
- create an UI image component (container for the sprite)
  - set native size
  - scale the bar from right to left: change the pivot x to 0 (Rect Transform)
  - prevent shrinking corners (squash look): slicing
    - In sprite editor
      - blue boundary: sprite extraction
      - green points: slice points
    - Image Type: Sliced
  - disable generate mip maps


## Coding


