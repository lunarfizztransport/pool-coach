# pool-coach

**Cue Coach: a pool strategy trainer. Call your shot, then see what a coach
would play and why.**

**Try it: [lunarfizztransport.github.io/pool](https://lunarfizztransport.github.io/pool/)**

I love playing pool, and strategy is the hard part: knowing which ball to
shoot, how to hit it, and when to play safe. This tool deals out a table and
asks you to make the call. Then a coach that has simulated every option grades
your call and explains its own.

## How it works

1. **Pick a game.** 8-ball (you're solids), 9-ball (lowest ball first, with a
   push out on the shot after the break), or **Kicks** practice.
2. **Make your call.** Click a ball, then a pocket. Choose spin (draw, stun,
   follow) and speed (soft, medium, firm, hard). Or press **Play safe**, or
   drag on the table to aim anywhere.
3. **Ask the coach.** It grades your call against every option it simulated
   and shows its pick on the table: the aim line, where the cue ball travels,
   and the next ball it sets up.
4. **Shoot.** Play your call or the coach's and watch it run. **Undo shot**
   steps back one shot at a time, and **Rack history** lets you revisit,
   replay or play on from any shot in the rack.

## What the coach is weighing

- **Which ball:** every shot is scored as *make chance × quality of the
  leave*. The easiest pot is often the wrong one if it leaves you nothing.
- **How to hit it:** for each pot it tries 3 spins × 4 speeds and keeps the
  one that leaves the cue ball best. It flags combos that scratch.
- **When to play safe:** if your best pot is under about 45% and a safety
  holds your opponent under 40%, it plays safe, and tells you which ball to
  hit, how thick, and how soft.

- **Push out (9-ball):** it looks for the push that leaves the closest to a
  50/50 shot, so your opponent's choice doesn't help them, and recommends it
  when your best pot is under 50%.
- **Kicks:** for a blocked ball it shows the mirror-system aim (the ball's
  reflection across the rail), the real aim after the cushion's effect, in
  diamonds, and which rail gives the most room for error.

After shooting you can ask the coach about your last shot, or review your
whole run: matches, patterns, the biggest leak, and every shot replayable.
**Hide coach** keeps it out of the way while you try to run out.

Make chance uses Dr. Dave Alciatore's margin-of-error formula: how far your aim
can be off and still pocket the ball, given the distances, the cut angle and the
pocket size, compared with a typical aim error for the **skill level** you pick.

## How it's built

One self-contained `index.html`: no build step, no framework, no server. The
table is drawn on a canvas.

- **Physics:** a 9 ft table (100 × 50 in), stepped at 400 Hz. Each ball slides
  until cloth friction turns it into natural roll, so the cue ball curves into
  its final line after contact and draw fades with distance. Rails bounce
  the ball off with stun.
- **Throw:** ball-to-ball friction drags the object ball off the ghost-ball
  line on cuts. The game, and the coach's aim, allow for it.
- **Spin buttons** set the spin at contact (stun, follow, draw). The hit
  needed for that distance is worked out, capped at the most spin a cue
  tip can give.
- **Coach:** for every legal ball and pocket it finds the aim that pockets the
  ball (allowing for throw), runs the full simulation for each spin and speed,
  and scores the leave by finding your best shot from where the cue ball
  stops. Safeties are simulated across 7 hit thicknesses × 3 speeds.

### Where the numbers come from

All from Dr. Dave Alciatore's technical proofs ([billiards.colostate.edu](https://billiards.colostate.edu)):

| What | Value | Source |
|---|---|---|
| Cloth rolling resistance / sliding friction | 0.01 / 0.2 | TP B.6 |
| Rail rebound | 0.7, ball rebounds with stun | TP B.6 |
| Ball-to-ball rebound | 0.94 | TP A.6 |
| Ball-to-ball friction (throw) | 0.00995 + 0.108·e^(−1.088·v), v in m/s | TP A.14 (fit to Marlow's data) |
| Make chance | margin of error = pocket size × 2R·cos(cut) ÷ (d₁·d₂); pocket size ≈ mouth − 2 in | TP 3.4 |
| Shot speeds | 1.5, 3, 5, 8 mph | TP B.6 |

Checked against those sources, rolling distances match within 5% and throw
curves within about half a degree.

**Estimates:** the skill levels (set so a long half-ball cut goes in about 25%,
45% and 75% of the time), how much a pocket shrinks at an angle, and the
strategy thresholds. **Not modeled:** left/right English, squirt and swerve,
jump and masse shots, banks and combinations as options, pocket rattles.

## Install it as an app

Open [lunarfizztransport.github.io/pool](https://lunarfizztransport.github.io/pool/) and:

- **iPhone / iPad:** in Safari, tap Share, then **Add to Home Screen**.
- **Android, or Chrome / Edge on a computer:** tap **Install app** (in Settings, or the browser's
  menu / address bar).

It opens full screen with its own icon and works offline. A small service worker (`sw.js`) saves
the game, always loading the newest version when you're online.

## Run it locally

Open `index.html` in a browser. The installable-app parts (`manifest.webmanifest`, `sw.js`,
`icons/`) only work when it's served over HTTPS or from `localhost`.

## On my website

The site at [lunarfizztransport.github.io](https://lunarfizztransport.github.io)
serves a copy of this page at `/pool/` and shows it in the **Pool Coach**
window. After changing `index.html` here, copy it over:

```bash
cp index.html manifest.webmanifest sw.js ../collage-site/pool/ && cp icons/*.png ../collage-site/pool/icons/
```
