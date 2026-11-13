# Four bodies, four parties

This page covers `tests/world_lib.gd` and `tests/world_gate.gd`: four people in one process, each
with a real body, each seeing the other three, and each sender judged by four parties. It is rung
2 of the ladder in `ledger.tsv`.

## The run

One physics guest holds four people on one model. Each plays a clip of its own, walk, idle,
walk back and run, forward and then backward so that it never jumps, standing 1.6 m apart. A
person sends on its own tick of the six, 0, 1, 3 or 4. A send is 21 Packets from the codec,
sealed by that person's observer with its hashes and its tags, and put into the router. Each of
the other three takes the two frames out and checks their size, ids and tag.

A frame that passes is held, not drawn. At the end of each interval of 6 ticks every observer
closes: it gives a verdict on each other sender from four reports, the sender's own hash, its
own, and the two other viewers'. A verdict of valid is about the send before, and only then do
that send's points go into the viewer's curves. A viewer draws 12 ticks behind, and a point
arrives in its curve one to five ticks before the view time reaches it, so nothing is drawn that
four parties did not agree on and nothing is shown and then taken back.

The run is 960 ticks, 15 seconds.

## What was measured

| Number | Value |
| --- | --- |
| Pairs of people who see each other | 12 of 12 |
| Verdicts that were valid | 1,896 of 1,896 |
| Party named | nobody |
| Mean distance of a drawn part from the sender's 12 ticks before | 1.60 mm |
| Largest distance | 160 mm |
| Largest distance between points of a rotation star | 165 mm |
| Share of a tick for one world's codec, router, observers and curves | 0.027 |
| Share of a tick for one world's physics | 0.66 |

A pair sees each other when all 21 parts were drawn and their mean distance is under 5 mm.

The largest distance is far worse than rung 1's 9 mm and the mean is seven times its 0.21 mm.
Two things are new in this run. One clip is a run, which is faster than a walk. And each clip
turns round at its end, where every velocity changes sign within one tick; a curve through
points 6 ticks apart cannot follow that, and no real body does it. The mean is the fairer
number and the largest is kept as it is.

The physics of one body takes two thirds of a tick on the interpreted tier, so four bodies do
not fit one process at 64 ticks a second. They are not meant to: each client has its own.

## The controls

Each control is planted from tick 300 and must be named.

| Control | What is planted | Named | Pairs seeing |
| --- | --- | --- | --- |
| `alter` | the zone flips one bit of slot 1's first frame for every viewer | `forged:1` | 9 |
| `halves` | the sender in slot 3 sends the second frame of the send before | `sender:3` | 9 |
| `stale` | viewer 2 keeps an old first frame of slot 0 | `viewer:2` | 9 |

In each case the three pairs that lose sight are the ones the planted party is part of, and the
other nine go on.

    godot --headless --path . -s tests/world_gate.gd -- <mujoco.double.elf> <clips dir> [--control=alter|halves|stale]

## Not here

The four as separate programs through the relay, which is the next rung. Avatars: this gate
measures the 21 parts and poses no skeleton, and the movie that does is still to be made. Any
placement of a send's work across ticks.
