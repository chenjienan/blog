---
title: 独立游戏开发 - Unity UI for Collectable Items
p: GameDev
date: 2020-12-26 16:26:28
category: gamedev
---

# Unity UI for Collectable Items

## Create an UI object

Comes with a canvas object. the Text object will be created as a child object to the canvas.

- canvas
  >how to display the text
  - reference resolution: choose the biggest one
  - UI Scale Mode: scale with screen size
- text 
  >set up your own test config
  - font size
  - Color
  - anchor preset (for different resolution)

## Coding

1. Player class stores an integer as a counter `int coinsCollected`
2. Player class stores a reference type Text to the UI text component `Text coinsText`

In the player object

``` C#
// get coin status at the beginning
void Start()
{
  coinsText = GameObject.Find("Coins").GetComponent<Text>();
  UpdateUI();
}

public void UpdateUI()
{
  coinsText.text = coinsCollected.ToString();
}
```

In the collectable object

``` C#
private void OnTriggerEnter2D(Collider2D collision)
{
  Debug.Log("touched!");
  //If the player is touching 'me'
  if (collision.gameObject.name == "Player")
  {
    GameObject.Find("Player").GetComponent<Player>().coinsCollected += 1;
    GameObject.Find("Player").GetComponent<Player>().UpdateUI();
    Destroy(gameObject);
  }
}

```
