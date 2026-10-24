# Car Park Jam

A 3D puzzle game for young kids (around age 5), made for phones and tablets. The car park is full and every car wants to go home, but they're parked so tightly that most of them are stuck. Tap the cars in the right order to let them all out.

- **Tap a car and it drives straight out**, forwards or backwards (it beeps when it reverses), whichever way is clear. Cars only ever go in a straight line out of their space. Once out, they turn on the spot and drive round the road to the barrier, which lifts for each one.
- **Cars never drive through each other** (see "How the traffic works" below). They queue, wait their turn to pull out, and take turns at the corner.
- **The half you tap goes first.** Tap near the bonnet to drive forwards, near the boot to back out. If that way is blocked but the other isn't, the car goes the other way. A swipe along a car picks the way it goes.
- **A stuck car bumps** into whatever's in its way and rolls back to its space. A car it bumps honks, looks cross and gets a 💢 bubble, a cone or tree wobbles, and a hedge shakes.
- **Nothing can go wrong.** A car that leaves only ever makes room, so no tap can make a car park impossible, and there's no timer, no score and no losing. Every car park can be emptied.
- **A hand shows the way** if nothing has left for a while: it points at a car that can go. On the first car park it shows up straight away.
- **Fifteen car parks that slowly get harder**: bigger, fuller, with long vehicles (buses, fire engines, lorries and cement mixers take three spaces), then cones, trees and bollards that never move, then hedges along some edges where no car can get out. After the list they keep coming with new layouts.
- **Sleepy cars** (from car park 4) have their eyes shut and Zzz floating up. The first tap wakes them with a yawn and a ❗; after that they drive like any other car. A car that bumps into a sleepy one wakes it up too. They're always cars that can't leave straight away, so waking them is part of working out the order.
- **Pop-up bollards** (from car park 9) sit in empty spaces on a car's way out. They sink into the ground, blink their red light for a second, then rise and stay up for a few seconds. A car can only get past while the bollard is down; one that tries while it's up bumps into it with a clang. A bollard never rises under a car that's on its way out.
- **Ducks** (from car park 3): every so often a mother duck and three ducklings waddle from the pond across the zebra crossing just past the barrier, and back again later. They wait at the kerb until no car is coming, then every car stops at the crossing until the last duckling is over. Tap them and they quack and hop.
- **Fun extras**: the police car, fire engine and ambulance flash their lights and play a siren as they leave, the ice-cream van plays a jingle, the cement mixer's drum turns, every car blinks, the cars lean back when they pull away and dip when they stop, puff a little exhaust, kick up dust when they bump, and leave a burst of stars as they pass the barrier. The badge at the top counts the cars still in the car park.
- **Five looks that take turns** (car parks 1 and 2 are always sunny): a sunny day, a golden evening, autumn with orange trees and falling leaves, snow with white roofs and snowflakes, and night with lit windows, lamps and headlights.

When the last car is out there's confetti and a cheer, and ▶ goes to the next car park. The game remembers which car park you're on. On the start card, a small ↺ in the corner starts again from car park 1.

🏠 in the top-left corner goes back to the list of all games, and the speaker in the top-right corner turns sound off and on.

## Run it

It's one page plus the sound clips in `assets/`. Serve the folder (for example `python -m http.server 5230`) and open it in a browser. It needs an internet connection the first time, for three.js and the font. No build step. Opening `index.html` straight from disk works too, but the browser won't load the clips, so you hear the synthesized sounds and the device's own voice instead.

## Play offline (add to Home Screen)

On iPhone or iPad, open the game in Safari, tap **Share → Add to Home Screen**, then open it once from the new icon while online. After that it starts full screen and works without internet. Changes arrive on their own: the next online launch downloads them and the one after shows them.

(How: `manifest.webmanifest` gives the icon and full-screen mode; `sw.js`, a service worker, keeps a copy of every file the game uses. The icons `icon-180.png` and `icon-512.png` are rendered from `icon.svg` in headless Chrome.)

## How the car parks are made

Each car park is made by the game from its level number, so car park 5 is always the same one. Cars are packed into every space, with the ones along an edge mostly lying along it so they block each other. Then cars that could never get out are taken away until the car park can be emptied, and some of the cars that could leave straight away are taken out too, so only a few can go at the start. Of 40 tries, the game keeps the one closest to the level's target. Car park 1 is drawn by hand: one car stuck between two that can go.

## Tuning knobs

At the top of the script in `index.html`:

- `LEVELS`: one line per car park: its size in spaces (`w` × `h`), the number of cars, `long` (the share of three-space vehicles), `obst` (cones, trees and bollards), `fence` (the share of edge spaces with a hedge) and `depth`: how many rounds it should take if every car that can go leaves at once (higher is harder).
- `REPEAT_FROM`: after the list runs out, new car parks are made from the levels from this one on.
- `DRIVE_SPEED`, `ROAD_SPEED`: how fast cars drive in the car park and on the road round it.
- `ACCEL`, `BRAKE`: how quickly cars speed up, and how hard they stop behind another car.
- `BUMP_SPEED`: how fast a stuck car rolls into whatever is in its way.
- `HINT_DELAY`: seconds without a car leaving before the hand points at one that can go (9). `FIRST_HINT`: the same on car park 1 (1.2).
- `PIVOT_RATE`: how fast a car turns on the spot when it pulls out onto the road.
- `STUCK_POOF`: a last resort the traffic rules shouldn't ever need: if no car at all has moved for this many seconds (6), the one that has waited longest vanishes in a puff of smoke rather than drive through another. It sends a `jam` event to the local telemetry, so it shows up in the report.
- `COLORS`: the car paint colours.
- `CLIP_VOL`, `MUSIC_VOL`: how loud the clips and voice are, and the music.
- `CLIP_TRIM`: each clip's own loudness, evened out from measuring them; frequent sounds (bollards, the barrier, the stars) are kept low.
- `HELP_TIMES`: how many times each helper line is said, ever, on a device (2).
- `MUSIC_XFADE`: seconds of each music track's end blended into its start (2).
- `ASSET_V`: bump it after replacing any file in `assets/`, so phones don't keep the old one.
- In `LEVELS`, `sleepy` (how many sleepy cars), `pops` (how many pop-up bollards) and `ducks` (whether ducks cross).
- `POP_DOWN`, `POP_WARN`, `POP_UP`: how long a pop-up bollard stays down (4 s), blinks before rising (1 s) and stays up (3 s).
- `DUCK_EVERY`: the shortest and longest wait between duck crossings (14 to 24 s). `DUCK_SPEED`: how fast they waddle.
- `THEMES` and `THEME_ORDER`: the sky, light, grass and tree colours of each look, and the order they come in.

## How the traffic works

Every car has an outline, a rectangle a hair bigger than its body, and a car only ever moves to where its outline doesn't overlap another car's, so cars can't drive through each other. Cars look ahead as far as it takes them to stop and brake for whatever is there, then creep up to it.

That rule alone could leave two cars each waiting for the other, so a few places are booked before a car may go in, and a booking is only given when nobody is in it:

- **Turning squares.** A car pulls straight out of its space to a turning point far enough out that it can turn on the spot without sweeping over the car park (long vehicles turn on the edge of the pavement, which is why the buildings, lamps and trees stand well back). Before it leaves its space it books a square of road big enough to turn in, so it never waits halfway out. If it has waited a second at the edge, road traffic stops driving into its square, so the square empties.
- **The corner** where the two streams of traffic meet, at the top of the exit road, takes one car at a time.
- **The zebra crossing** is booked by the ducks while they cross.

On the road, a point near each car's front and one near its back both follow the road, like real wheels, so long vehicles cut a little inside a bend instead of swinging their back end out.

Tested in simulation over about 80 runs across every car park, from a tap every 1.5 seconds to every car tapped at once: car bodies never overlapped, no duck was touched, the last resort never fired, and at a child's pace no car waited more than about 3 seconds.

## How it's drawn

Everything is built in code with three.js, with no picture files: the cars are side outlines pushed out to their width with softened edges, given wheels, lights and cartoon eyes on the windscreen. The ground, roads, car park and paving are painted on canvases when a car park starts. To keep phones smooth, each car's plain coloured parts are baked into one mesh, and so are all the buildings, trees, lamps and flowers. A full car park draws in about 250 calls. Phones get a smaller shadow map.

## Sound

Recorded clips (see Audio credits below) for the engines, horns, bumps, bollards, the barrier, the sirens, the ice-cream jingle, the sleepy yawn, the ducks, the sparkle at the barrier and the end fanfare, plus a gentle music loop (a soft one at night) that dips while the voice speaks. A friendly voice cheers at the end of each car park and says four helper lines, each only the first two times it's needed on a device: "Tap a car that can drive out!" (car park 1, with the hand), "Shh, it's asleep. Tap to wake it up!" (the first car parks with a sleepy car), "Wait for the bollard to go down!" (after bumping a raised bollard) and "Wait for the ducks!" (when they cross in front of a car).

Every sound has a synthesized stand-in (Web Audio) that plays if its clip can't load, for example when the page is opened straight from disk; the voice then falls back to the device's own voice. The tap blip, the reversing beep, the hedge and cone boing and the pop as a car leaves are always synthesized.

## Audio credits

Made with ElevenLabs on 2026-10-24 on the paid **Creator** plan, which allows commercial use and needs no credit. Music made with Eleven Music may not be used in a game that's sold or makes money in any other way, so this game stays free with no ads. The clips and music are not covered by any licence on this game's code and aren't licensed for reuse.

- **Sound effects** (`assets/sfx-*.mp3`, 17 clips): ElevenLabs Sound Effects, three takes of each, best one kept: `sfx-start` (small engine), `sfx-start-big` (bus engine), `sfx-honk`, `sfx-horn-big`, `sfx-bump`, `sfx-bollard`, `sfx-clang`, `sfx-gate`, `sfx-siren-police`, `sfx-siren-fire`, `sfx-siren-ambulance`, `sfx-jingle`, `sfx-yawn`, `sfx-quack`, `sfx-peep`, `sfx-sparkle`, `sfx-fanfare`.
- **Voice** (`assets/say-*.mp3`, 7 clips): voice "Jessica - Playful, Bright, Warm" (a stock ElevenLabs voice), model Eleven Multilingual v2, stability 0.6, similarity 0.9, speed 0.95. All seven lines were recorded together in one take and cut apart at the pauses: `say-out-1` to `say-out-3` (the cheers), `say-tap`, `say-sleepy`, `say-bollard`, `say-ducks`.
- **Music** (`assets/music-day.mp3`, `assets/music-night.mp3`, 45 s each): Eleven Music. Day: ukulele, glockenspiel and light percussion; night: music box and soft pads. Each loops by blending its last 2 seconds into its start.

## For testing

`?mute` (or `?tbxtest`) on the address keeps the game silent without changing the saved sound setting, `?level=7` starts on car park 7, and `?theme=night` (or `day`, `evening`, `autumn`, `snow`) picks the look. From the browser console, `carpark.go(n)` jumps to a car park, `carpark.solve()` lets the cars out one by one, `carpark.tick(seconds, draw)` runs the game clock by hand (a hidden tab barely draws frames; `draw` false skips drawing, for long test runs), `carpark.depth()` tells how hard the current car park is, and `carpark.why(id)` says what's in the way of a car's next move.
