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

After every tag the new catcher is frozen for five seconds, so the one who just
got away has a real head start. Only the catcher is held — the escapee keeps
their momentum and runs off immediately.

## Picking a side

The start screen shows nothing but a flower and a banana: *which one are you?*
Tap one, tap START. Whichever you pick, you are always the lighter brown
monkey, so you can find yourself in a scramble even when both items are
recharging.

## Two players

The first two people to open the page play against each other — there is no
room code and nothing to join. A line under START says what is going on: alone
means you play the computer, otherwise it tells you Player 2 is here. Once the
other person picks a symbol, theirs is framed in green and the one they took
can no longer be chosen. Anyone arriving third is told the game is full.

This runs on Supabase Realtime. Presence tells each browser who else is on the
page; the rest goes over broadcast messages. The publishable key sitting in
`index.html` is meant to be public — that is how Supabase client apps work.
No database tables and no security rules are involved.

Whoever arrived first is the authority: that browser runs the simulation for
both monkeys and sends the result about fifteen times a second. The other
sends only its thumb input, twenty times a second, and predicts its own monkey
locally so the controls feel immediate despite the round trip; its position is
then eased back toward the authoritative one. Tags are decided in one place
only, so the two screens never disagree about who caught whom.

If someone closes the page mid-round, the other gets "Player 2 left" and the
round ends. This is deliberately crude — it is a toy, not a service.

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

- **White flower**: whoever walks into it stops to sniff, because it smells so
  good.
- **Banana**: whoever walks into it slips, stops dead, and loses their grip —
  step on one while hanging from a vine and you drop.

Both stop the victim where they stand. An earlier version had the banana fling
you along with momentum, which backfired: during a chase it slid the catcher
straight into the runner instead of costing them time.

Your own item never hurts you, and it lands on the side you are facing — the
same side you carry it on.

A catcher who cannot move cannot catch. While the heart-carrier is frozen,
sniffing, or slipping, you can walk straight past them.

## Vines

Once you are on a vine you stay on it. A thumb held at a slight angle used to
peel you off and drop you, so sideways movement is ignored while climbing and
you are pulled to the middle of the vine. Letting go is a deliberate sideways
push with no up or down — or pressing down at the very bottom, where there is
nowhere left to climb.

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
