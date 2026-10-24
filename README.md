# Catnip FM · catnipfm.com

Catnip FM is a little rooftop radio that stays up late. Loafie hangs out above the city while lo-fi, house, ambient and 8-bit are generated live in your browser. Tune around, pet the cat, or stack a few parts and save them to a mixtape.

Pet Cats and Chill.

This file describes the current build: what's in it, how it fits together, and the few compatibility contracts a developer needs to know. It is a reference, not a changelog.

## What's here

Plain static files. No build step, no server code, no API keys, no location.

| File | What it is |
|---|---|
| `index.html` | The whole site on one page: the radio and the deck, then Side A (Pet Cats and Chill, Make a mixtape), then Side B, the liner notes (`#notes`, nerd stuff at `#nerd`). |
| `privacy.html`, `terms.html`, `404.html` | Their own pages, each with the same header and board, so the music keeps playing between them. |
| `studio.html` | Builds broadcast addresses for OBS and recordings. Not linked from the site, not indexed. |
| `board.js` | Everything: the LED board, Loafie, the stations, the mixtape engine, the deck, broadcast mode, Float, page swapping. |
| `board.css`, `site.css` | The radio and deck; the page around them. |
| `analytics.js` | Google Analytics 4 (on; see the file's header to switch it off). |
| `sw.js`, `manifest.webmanifest`, `icons/` | Installs as an app and works offline. |
| `fonts/` | Doto and IBM Plex Sans, self-hosted (SIL Open Font License). |
| `og.png` | The link preview image. |
| `CNAME`, `robots.txt`, `sitemap.xml` | Hosting and search. |

`index.html`, `privacy.html`, `terms.html` and `404.html` repeat the header and board on purpose: every page works on its own, search engines get a complete document, and the in-site page swap (below) keeps the board alive. Don't template them away at runtime.

## The one rule: never break the flow

Catnip FM is a joyful, casual toy. Nothing anyone does here should knock them out of the vibe. When adding or changing anything, check it against these:

- **Never refuse.** An action does what it says. Saving always saves (at the 50-tape limit the oldest tape quietly makes room); nothing answers a tap with an error, a limit or a form.
- **Nothing opens that wasn't asked for.** A key that changes something changes it in place (DISPLAY steps to the next look; it doesn't open a tray). The only thing that changes shape is the tape deck, when you press REMIX.
- **Don't talk over the music.** The board's station line stays the station line: no toasts, no name flashes, no "done!" messages. The change itself is the feedback (a key lights, the board recolours, the tape changes).
- **No dead ends and no steps.** Every key is live (lit = playing, unlit = available), there's no wizard and no confirm screen, and the only question ever asked is "tap again to delete".
- **No clutter on the board.** Nothing big hangs in the sky or over Loafie (there's no moon).
- **Subtract before adding.** The product is: put on music, pet the cat, mess with a few layers, save a tape. Anything that doesn't serve that goes.
- **Music keeps playing** through everything: changing the mix re-cuts with a crossfade, never a stop.
- **No keyboard shortcuts**, on purpose: people type in other windows, and a stray key should never stop the music. Every control is a normal button, so Tab and Enter work.

## A first visit

The display is 808, the tuner is on Nine Lives (87.9, first on the dial). If the browser allows sound without a tap, it plays straight away; otherwise it waits, tuned to Nine Lives, and the first tap anywhere on the board or the case starts it (after that, tapping the board is for Loafie, and stopping the music stays stopped). The clock blinks 12:00 for a few seconds, like a radio just plugged in. Loafie is peeking over her box, hops out, and walks onto the ledge; a "Pet Loafie" bubble follows her until the first pet. Returning visitors get whatever they had: station, display, coat, volume, tapes (`localStorage` key `catnipFM`); a saved station that no longer exists falls back to Nine Lives.

## The board

One LED board, 128 dots wide: the clock on top (32 rows) and the rooftop under it (18 rows), with no gap between them. The dots are round pegs with a glint that scales with brightness. Behind both is the city: a night skyline in two depths, lit windows that come and go, a tall tower with a blinking red light, a few twinkling stars, and rain when it rains. Loafie's ledge is a plain grey concrete sill in front of it, the same in every palette. It's always night.

**While nothing plays** the clock side shows the time, the date, and what Loafie is up to. **While something plays** the mix is the hero: the time and frequency shrink to the top line, the track name takes the big letters, and a 16-band LED meter runs under it (bars snap on the beat, orange and red at the top, peak caps that hang). Loafie listens to the actual sound: on a kick she dips, on a hat she flicks an ear.

**Video credit.** When a station or tape starts, the clock side shows a late-night-video-show credit for about 3.6 seconds a page: for a station, LOAFIE / "Title" / album / CATNIP FM RECORDS; for a tape, its name, frequency, parts and MIXED BY whoever made it (MIXED BY LOAFIE if nobody did). Not for calm sounds, not while the deck is open. In broadcast mode it comes back every eight minutes and tapes get a second page, MAKE YOURS AT catnipfm.com.

**Late Night Video.** Friday and Saturday from 11 PM to 4 AM (board time), the skyline's outline is traced in neon, pink far and cyan near, with a light running along it, and the STEREO lamp reads LATE NIGHT VIDEO. `?late=1` forces it, for recordings.

**Three looks**, stepped through by the DISPLAY key: **808** (`pegs`, the default: red, orange, yellow and cream pegs; the city is putty-grey), **VFD** (`teal`: cyan and orange), **Red LED** (`red`: the 1987 clock radio). Until you pick one, the display follows the starting track of what's playing (Lo-fi → Red LED, House and 8-bit → 808, Ambient → VFD). After dark it dims on its own. Four older looks (`amber`, `crt`, `moon`, `ember`) still render for anyone who chose them, in broadcast links and in Studio's *Older looks*; they aren't offered on the DISPLAY key.

**Test switches** (query string): `?sky=rain` forces rain, `&night` the night-time states, `&late=1` the late show, `&knock=1` the mug, `&morse=1` and `&visitor=1` the other rare events (below). The console prints a short service-hatch note with the DEAD AIR hint.

## Loafie

She wanders, sits, loafs, grooms, yawns, glances about, stretches, slow-blinks, and dozes off when it's late or when she's been petted past six hearts. Rarely, in the daytime, zoomies: a dash to one end of the ledge, the other, and back. She has a cardboard box at the left end of the ledge; she climbs in, peeks out, ducks now and then, and sometimes her eyes slide over to the right for a couple of seconds.

- **Pet her**: tap and hold. She purrs (a real purr in the audio), hearts float up, and after a while she makes biscuits.
- **Call her over**: tap the ledge and she mews and walks there. Tap the box and she ducks, or comes and climbs in.
- **Fish**: the orange fish key drops a goldfish near her (up to four). She trots over, eats it purring, then sits happy and full.
- **Z**: she curls up for a nap (in her box if she's in it) and the board dims; tap Z, the board, or the fish and she wakes, yawns and stretches.
- **Coats**: the cat key cycles grey tabby, orange tabby, white, calico, Siamese, black. The same nine-row head is used on the board, the cassette labels, the coat key and the logo.
- **The mug.** There's a mug on the ledge. Once in a long while (about one in 160 of her idle choices, never in the first minute and a half) she walks over, stares, pats it twice, pushes it off the roof, watches it go and slow-blinks at you; a faint tink comes from far below. Another mug turns up 20 to 40 minutes later. No counter, no explanation.
- **Away**: leave the page ten minutes and she curls up; come back and she wakes, stretches and wanders over.
- **Rarer still, no UI:** the tower's red light blinks M E O W in Morse for a few seconds (`MORSE`, about one idle choice in 300); an orange cat walks in from the far right of the ledge, sits a moment, and leaves (`visitor`, about one in 500, never during the mug). `?morse=1` and `?visitor=1` bring them on at once, for recording.

On a touch screen the pet target is a thumb wide. Reduced motion skips the entrance and the zoomies.

## The radio

A digital tuner: ◀ ▶ step through the band, FOCUS / CALM / TAPES are the band keys. Mute, volume (computers only; phones have buttons), DISPLAY, the coat key, the fish, Z, and FLOAT (computers only).

**FOCUS**, three stations, each with a steady beat and its own flavour. Nothing is a recording: the music is composed as it plays.

| Station | Freq | Sound | How |
|---|---|---|---|
| Nine Lives | 87.9 | Mellow 8-bit with a late-night video-show sheen: chip drums with a real clap on two and four, triangle bass, pulse arps, a soft chorused pad (C major). First on the dial, the default. | tape recipe (`STATION_RECIPE`, seed 18) plus `STATION_OPTS` `sheen` |
| Late Shift | 88.3 | Lo-fi hip-hop: Rhodes, dusty boom-bap, lazy swing. | the older `music()` conductor with its `STYLES` entry |
| Rooftop | 94.1 | House: four on the floor, synth bass, pad. | tape recipe, seed 1; internal id `gen:crt` (legacy id, public name Rooftop) |
| Today's Tape | 91.3 | A recipe and a seed picked from the date (`todayTape()`), so everyone on Earth hears the same tape that day; the deck and the credit show the date. Turns over at midnight. No streaks, nothing saved. | `gen:today` |
| DEAD AIR | 83.3 | Not on any band. Press ◀ below Nine Lives (the bottom of the dial), or `?station=deadair`. A slow drone under static, the band readout blank, Loafie sitting facing away, staring at something off the edge of the roof. ▶ brings you back. Not explained anywhere. | `gen:dead`, `HIDDEN` |

**CALM**: Purr 89.9, Rain 90.5, Ocean 93.3, Fireplace 99.1, Brown noise 100.7. A sound machine, no songs.

**TAPES**: your saved mixtapes, each on its own frequency.

Frequencies are fictional (`FREQ` in board.js); never pair them with call letters. Every sound is synthesised with the Web Audio API: there are no samples and nothing to license. Levels are matched by measured loudness (`GEN_LEVEL`, `TAPE_LEVEL`) so switching never jumps.

## Mixtapes

**REMIX** is the one word that opens the deck, on a station, on your unsaved tape, on a saved tape, on someone else's tape. **NEW TAPE** sits beside it in every now-playing view: the current station's groove, nothing else carried over, a fresh tune. The deck slides the cassette in (a bottom sheet on a phone, with Loafie still in view) with music already playing. From a station it starts with that station's exact parts and a fresh tune.

The keys: **START WITH** (Lo-fi, House, Ambient, 8-bit: the groove, drums and tempo), **SPEED** (Slow, Easy, Fast; always one of the three; picking a groove sets its own), **BASS** (Warm, Deep, Synth), **KEYS** (Rhodes, Guitar, Pad, 8-bit), **OUTSIDE** (Rain, Purr, Fire; Ocean stays on a tape that has it). Lit means on; tap a lit BASS, KEYS or OUTSIDE key to take it out. Every key press re-cuts the tape with a quick crossfade. **NEW TUNE ↻** (big, yellow, next to SAVE TAPE) rolls a new seed: same parts, a new key, mood, chords and hook. Under the cassette, ON THIS TAPE shows the last bar's kicks, snares, hats, bass and key hits (`keyHit()` logs melodic notes for it) with a 16-step light run in the 808 colours.

**SAVE TAPE** names the tape from its parts and the time of day ("Warm Saturday Night"; the name follows your changes until you save, then stays unless you RENAME), keeps the shell you were looking at, gives it a free spot on the dial, and pops it out as *now playing*. Up to 50 tapes; the oldest makes room. The unsaved draft survives a refresh.

**SHARE** copies the link (and shows it, so you can select it). **FROM (OPTIONAL)**, u/yourname: plain text, 24 characters, never verified, just what somebody wrote on the cassette label. It's remembered and rides along on every tape you share. **Save as picture** makes a 1200×630 PNG of the tape, since link previews can't run the page. Tap the cassette for another shell (14 of them, 80s tape-rack looks, no real brands).

**Someone else's tape** (a `?tape=` link) plays straight away and says MIXED BY their name. The big key is **REMIX**: it becomes your own copy, and it remembers whose it was (`from`, one generation), so when you save and share it the deck and the credit say MIXED BY you · REMIXED FROM them. KEEP THIS TAPE keeps it as is, with their name.

**COPY REDDIT POST** (on a saved tape) copies plain text to paste under a thread: `u/you — Name`, the parts, the frequency, `remixed from u/them` if so, and the link. Every cassette carries a serial number, CN- plus five digits from the seed: meaningless, the same for the same tape, and somebody will ask whether low numbers are rare.

### The engine (`band()`)

Each tape is a recipe plus a seed. The seed picks the key, one of three moods (`HARMONY`: diatonic scales with 7th and 9th chords), two chord progressions (the A and B halves of each 32 bars) and a two-bar hook (`makeHook()`). The B half gets its own hook, and every 64 bars a new one comes in, so a tape left playing for an hour keeps moving while still sounding like itself. The hook never repeats verbatim: as written, then a new last note, then a rest while the keys improvise, then a step higher. Every melody note is pulled onto the chord underneath it (`fit()`), which is why any combination sounds right. The drums set the feel (soft and lo-fi share the lo-fi feel; house is four on the floor with offbeat hats and a gentle pump; no drums plays freely, and with Pad keys becomes ambient); the 8-bit start puts the drums through a chip kit. Fills go into each new section; every 32 bars there's a four-bar breakdown. Lo-fi drums go through a quiet cassette (a little hiss and wow, `RECORDING[1]`).

### Tape schema and links

A tape is `{n, c, d, b, k, w, p, f, s, r, e:5, i:1, by?, from?}`: name, shell, drums (0 off, 1 soft, 2 lo-fi, 3 house), bass, keys, outside, speed, frequency × 10, starting track, seed, schema version, independent layers, optional credit, optional remixed-from credit. The link is `?tape=` + base64url of `[7, n, c, d, b, k, w, p, f, s, r, by?, from?]` (`encodeTape`; `by` is written as `''` when only `from` is set). Nothing is stored anywhere: the link is the tape, and whoever opens it gets the same music. `decodeTape()` also reads versions 1 to 6 (older fields are mapped onto the current ones in `cleanTape()`); this is compatibility, not dead code. Older seedless tapes use their name as the seed under the `'loaf:'` prefix: that prefix is a compatibility string, not branding, and changing it would change their tunes. In independent-layer tapes (`i:1`, everything made now) the starting track alone sets the drums, groove and tempo, so House + Synth bass + 8-bit keys stays house drums at house tempo.

## The page and the deck

The radio is a piece of 1983–1988 consumer electronics in a dark room, and it's always night: there's no light mode (`applyScheme()` is dark only, whatever the device is set to). One warm charcoal faceplate (`--plate`) for the radio and the deck, framed by a hard-edged pixel bevel with corners stepped like a sprite on desktop (square on phones so the deck's bottom sheet isn't clipped). The display is dark glass; colour on a key means it's on. Body text is warm off-white, never pure white on black.

**The keys** follow one rule: lit means on. A key that's on fills flat with its colour (the 808 orange for BASS, KEYS and OUTSIDE; amber for the active SPEED segment) and its legend goes dark; a key that's off is dark plastic. Pressing is only the moment the finger is down, a 1px dip, then it comes back up, so nothing in the resting state looks sunk. START WITH keeps the four 808 colours: the chosen groove at full colour with a dark bar under it, the other three dimmed to about two thirds. NEW TUNE is the yellow function key; SAVE TAPE is the biggest key, orange, with the REC lamp beside it. Keys are square with a 1px highlight on top and a 1px shadow underneath, no gradients, nothing glowing on the plastic. The beat display's lamps are set into the panel, grouped in fours with 1, 5, 9, 13 printed under the light run; the playhead is a faint column sweeping all five rows, a lamp on the current step flashes bright and fades over the next step (a CSS transition), and the run's lamp leaves a short trail. The volume fader is a dark recessed slot with a chunky ridged thumb over printed tick marks. The chassis is warm dark plastic with four corner screws and a seam between the radio and the deck; the deck header prints RHYTHM COMPOSER while you mix.

Every key is at least 44 px square (play is 52); text people need to read is 12 px or more; only decorative print is smaller. On a phone the now-playing cassette sits beside its name so REMIX is on the first screen, and in the deck the keys come first with the pattern display and the cassette under them.

The page around the radio is a cassette J-card: the 80s stripes (`--s1 #ef2a0c`, `--s2 #f2520f`, `--s3 #f6860b`) on the logo, the sleeve, the A1 liner note and the footer; the privacy, terms and 404 pages open with the same J-card header.

**Page swapping.** Links inside the site fetch the next page and swap in its `#page` below the board, so the cat and the music carry on; the address, title and history update, and Back works. Every page is still a complete HTML file for direct loads, new tabs and search engines.

**Float** (computers): Chrome and Edge use Document Picture-in-Picture, so the real clock and rooftop move into an always-on-top window with the fish, Z, display and mute keys under it; petting works there. Safari gets a video picture-in-picture of the board. Phones don't offer it, so the key is hidden.

## Broadcast mode (YouTube videos and livestreams)

`?broadcast=video` (no clock or date; the big digits show the frequency) or `?broadcast=live` (the real time on the small line, in one time zone) turns the page into just the board on a dark background, sound starting by itself where the browser allows it, Loafie never dozing off, a treat dropping now and then.

Options: `station=nine|late|crt|today|deadair|purr|rain|ocean|fire|brown` (`rooftop` works too), `stations=nine,late,crt&rotate=30` (take turns every N minutes with a fade), `tape=…` (one mixtape), `tapes=code,code,…` (community tapes, up to 60, taking turns with the station(s); rotate defaults to 15), `by=u/name` (the MIXED BY line, overriding the tape's own), `palette=pegs|teal|red|amber|crt|moon|ember`, `coat=grey|orange|white|calico|siamese|black`, `tz=America/Los_Angeles` and `label=PT` (live), `vol=0.8`, `treats=0`, `dim=0` (no evening dimming), `late=1` (neon), `outside=rain|purr|fire|ocean` (a layer under any music station, as on a tape; rain also rains on the board; skipped on the calm stations and on tapes that have their own), `frame=vertical` (9:16: the board on top, the playing cassette with its name and frequency under it, catnipfm.com at the bottom; record at 1080 × 1920).

Broadcast mode never touches your own saved settings.

**Studio** (`/studio.html`) builds these addresses with a live preview and a copy key. It opens on Nine Lives with the neon late show and no dimming. Quick starts: *Nine Lives late show* (16:9, neon), *Late Shift session* (16:9, Red LED), *A Short* (9:16, Nine Lives); none of them touch *Outside*, so rain or a purr stays on when you switch. Paste one listener's link to feature a tape, or several into *Community tapes* for the stream. Studio keeps its settings under `localStorage` key `catnipFMStudio2`.

## Made for Reddit

The loop: make a tape → put your u/name in FROM → COPY REDDIT POST → people play each other's tapes (every one says MIXED BY) → somebody hits REMIX and their tape says REMIXED FROM yours, so the thread branches → good ones go on the stream as community tapes → viewers see MAKE YOURS AT catnipfm.com. Today's Tape gives everyone one thing to talk about; DEAD AIR and the rare rooftop events give people something to find and report back. No accounts, no profiles, no social network. Share links are plain `?tape=` addresses with nothing tracked.

**What gets counted** (GA4 events from `track()` in board.js, never in broadcast mode): `play_radio` (with `station`), `pet_loafie`, `mixtape_open` (once per visit each), `mixtape_save`, `mixtape_share` (every time), `tape_link_open`. Mark the last three as key events in GA4. The funnel that matters: visitors → play → pet → open the deck → save → share.

## Analytics

Google Analytics 4 is on, Measurement ID `G-QY4M7NZD76` in `analytics.js`; the privacy page describes it. To switch it off, empty the ID and deploy. Advertising storage is always denied; in the EEA, the UK and Switzerland analytics storage is denied too (cookieless pings), and nothing loads under Do Not Track or Global Privacy Control, or on localhost. In the GA4 data stream, leave enhanced measurement's "page changes based on browser history events" on (that counts swapped pages), turn Google Signals off, and set data retention to the minimum you need.

## Put it online

**GitHub Pages**
1. Create a repository and upload everything in this folder (so `index.html` is at the root).
2. Settings → Pages → Deploy from a branch → `main`, `/ (root)`.
3. Settings → Pages → Custom domain: `catnipfm.com` (the `CNAME` file is already here). Tick *Enforce HTTPS* once it's offered.
4. At your registrar, point the domain at GitHub Pages (four `A` records for the apex plus a `CNAME` for `www`; GitHub's Pages docs list the current addresses).

Cloudflare Pages or Netlify work the same way. HTTPS is required for installing it as an app. After launch, add the site in Google Search Console and submit `https://catnipfm.com/sitemap.xml`.

**Updates**: upload the changed files. The service worker fetches from the server first (revalidating rather than trusting the browser cache), so anyone online gets the new version on their next visit and installed copies refresh their offline files as they go. The cache name (`catnipfm-v1` in `sw.js`) only needs changing if `sw.js` itself changes how it caches.

**Before adding ads or affiliate links.** AdSense sets advertising cookies, needs the privacy page to describe them, and in the EEA, the UK and Switzerland needs a Google-certified consent banner; `analytics.js` already denies advertising storage, and a banner would update consent. Amazon Associates needs "As an Amazon Associate I earn from qualifying purchases." on the site and a short disclosure next to the links; write your own descriptions, and keep commerce on a normal page, not inside the board.

## Compatibility contracts

Things with historical names that must not be renamed, because saved state, links and broadcast addresses depend on them:

- `gen:crt` is Rooftop's internal id (`station=crt` and `station=rooftop` both work).
- `pegs` is the 808 display's internal id; `teal` is VFD, `red` is Red LED.
- Tape link versions 1 to 7 all decode; the `'loaf:'` seed prefix for seedless tapes stays.
- The `?tape=` field order `[7, n, c, d, b, k, w, p, f, s, r, by?, from?]` and the frequencies in `FREQ`.

Storage keys are `catnipFM` (the site) and `catnipFMStudio` (Studio); the session flag for Loafie's entrance is `catnipIn`; test hooks are `window.catnipBand`, `window.catnipSounds` and `window.catnipCard`.

## Contact address

`hello@catnipfm.com` appears in the footer, the home page and the privacy and terms pages. Search and replace if you use a different one.

## Credits

Music and sounds: Catnip FM's own code, played in your browser. Fonts: Doto and IBM Plex Sans (SIL Open Font License). The cat and the board: made for Catnip FM.
