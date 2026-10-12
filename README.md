# pool-coach

**Shot Caller: a pool strategy trainer. Call your shot, then see what a coach
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
4. **Shoot.** Play your call or the coach's and watch it run. **Retry last
   shot** puts the balls back so you can try the other way.

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

Make chance is estimated from cut angle, cue-to-ball distance and
ball-to-pocket distance (plus a penalty for steep side-pocket angles).

## How it's built

One self-contained `index.html`: no build step, no framework, no server. The
table is drawn on a canvas.

- **Physics:** a 9 ft table (100 × 50 in) with rolling friction, cushion
  rebounds, ball-to-ball collisions and pockets, stepped at 400 Hz.
- **Spin:** after first contact the cue ball keeps its tangent speed, then
  friction blends in its spin: `v = 5/7 · v_tangent + 2/7 · spin · v_before`.
  This gives the 90° rule for stun and the forward/back bend of follow and
  draw. Draw weakens over distance.
- **Coach:** for every legal ball and pocket it aims with the ghost ball,
  runs the full simulation for each spin and speed, and scores the leave by
  finding your best shot from where the cue ball stops. Safeties are
  simulated across 7 hit thicknesses × 3 speeds. A full analysis takes about
  0.3 seconds.

**Simplified on purpose:** no left/right english, throw, jump or masse
shots. Trust the ranking of options more than the exact percentages.

## Run it locally

Open `index.html` in a browser. That's it.

## On my website

The site at [lunarfizztransport.github.io](https://lunarfizztransport.github.io)
serves a copy of this page at `/pool/` and shows it in the **Pool Coach**
window. After changing `index.html` here, copy it over:

```bash
cp index.html ../collage-site/pool/index.html
```
