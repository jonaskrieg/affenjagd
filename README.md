# Monkey Chase

Tag in the treetops. Two pixel monkeys chase each other up and down a tree —
built for the iPhone, playable with one thumb in portrait, shareable as a link.

**Play it: https://jonaskrieg.github.io/affenjagd/**

## Rules

Whoever wears the red heart is *it* and has to catch the other one. A touch
passes the heart on. **You only score while you are not it** — 10 points per
second of freedom, plus 25 for tagging your way out of it. There is no clock:
the round runs until each monkey has been it three times. The three hearts next
to `YOU` and `HIM` in the top bar track that.

After every tag the new catcher pauses for a moment, so the one who just got
away has a head start.

## Controls

Put your thumb anywhere and drag — the stick appears wherever you touch.

| Input | Effect |
|---|---|
| left / right | run |
| up on a vine | climb |
| up on a branch | jump |
| down on a vine | climb down |
| down on a branch | drop through (the fast way out) |
| button, bottom left | drop your item |

A second finger anywhere on the screen also drops the item.
On a computer: arrow keys or WASD, space for the item, enter to start.

## The two items

Each monkey carries its own item visibly in front of it. An empty hand means it
is recharging — that doubles as the cooldown indicator, so there is no extra
gauge.

- **White flower** (you): the other one stops to sniff it, because it smells so
  good. Buys you distance.
- **Banana** (him): you slip and lose control for a moment. Good for making
  someone overshoot.

Your own item never hurts you.

## How it is built

Everything lives in `index.html` — no build step, no dependencies, no external
files. The graphics are 16×16 sprites written as character grids right in the
source and scaled up with smoothing switched off. The tree is generated from a
fixed seed, so it is identical every round, and its height adapts to the
device's aspect ratio.

Branches are one-way: solid from above, passable from below. That is what makes
climbing through them and dropping through them work without holes in the
geometry — an earlier version used holes and monkeys kept snagging on the edges.

The opponent navigates a graph over the tile grid: branches allow sideways
steps, vines allow steps upward, and downward is allowed nearly everywhere. A
breadth-first search from the player produces a distance field which the
opponent follows — downhill when chasing, uphill when fleeing.

`icon.png` is generated from the sprite in `index.html` and serves as the
"Add to Home Screen" icon as well as the link preview image.

## Publishing

Served by GitHub Pages from `main` / `/ (root)`. The `og:` tags in `index.html`
point at the absolute Pages URL — WhatsApp ignores relative paths, so if the
repository is ever renamed or moved, those have to move with it or the link
preview disappears.
