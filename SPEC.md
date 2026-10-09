# Tarot Buddy — App Specification v1

**Owner:** Thomas Herreras (Tucson, AZ)
**Purpose:** An accessibility-first virtual tarot deck, designed so blind and low-vision readers can work with the cards independently. Monetized with ads to generate revenue for the Starving Artists Against Hunger (SAAH) fundraiser.
**Status:** Spec drafted Oct 9, 2026. No code written yet.

---

## 1. Why this exists

The app is named for Thomas's cousin Buddy, who has been blind since birth.

Tarot is an intensely visual practice. The entire tradition rests on imagery — a woman with her hands on a lion's jaws, a child on a white horse under a blazing sun, a magician with the four suits laid out on his table. Readers learn by looking, and the meanings live in the pictures.

Which means tarot has been, in practice, close to unavailable to blind readers. Braille decks exist but are rare and expensive. Most tarot apps are built as galleries of images with text bolted on, and are poor to unusable with a screen reader.

Tarot Buddy inverts that. It is built screen-reader-first, with the audio experience as the primary interface and the visual one layered on top. Sighted users get a normal, attractive tarot app. Blind users get the first one genuinely built for them.

## 2. Design principles

**Audio is the primary interface, not an accommodation.** Every element of the reading works with the screen off. Nothing requires sight at any point.

**Describe the picture, not just the meaning.** This is the heart of the app. A blind reader should not merely be told "The Sun means joy." They should be told what is on the card: the child, the white horse, the banner, the sunflowers, the wall, the blazing sun with a human face. The imagery *is* the tradition. Withholding it and substituting a keyword list is the exact failure this app exists to correct.

**Ritual matters.** Shuffling, cutting, and drawing are part of the practice, not loading screens. They must be physical, tactile, and performed by the user, not animated at them.

**Plain language.** No jargon without explanation. A first-time reader and a thirty-year reader should both be able to use it.

**Low vision is not the same as blind.** Many users have partial sight. High contrast, large scalable type, and no meaning ever conveyed by colour alone.

## 3. Accessibility specification

This section is the core of the product. It is not a checklist to be completed at the end.

### Screen reader

- Full TalkBack (Android) and VoiceOver (iOS) support, tested on device, not simulated
- Every control labelled meaningfully. No unlabelled buttons, no "button 14"
- Logical focus order through every screen
- Live regions announce state changes (deck shuffled, card drawn, reading saved)

### Built-in narration

Beyond the screen reader, the app has its own narration so a reading can be listened to as a continuous experience:

- Platform text-to-speech (free, built into both Android and iOS). No third-party voice service required.
- Adjustable speech rate and pitch
- Pause, resume, repeat, and skip back by card
- Option to narrate: card name only / name plus imagery / full reading

### Physical interaction

- **Shuffle by shaking the phone.** Accelerometer-driven, with an audible shuffle sound and continuous haptic feedback while shaking. The user shuffles for as long as they want and stops when it feels right.
- **Cut the deck by swiping** across the screen. Haptic confirmation.
- **Draw by tapping.** Each draw gives a distinct haptic pulse and a card-flip sound.
- All touch targets a minimum of 48dp, generously spaced, usable without looking.

### Visual accessibility

- High-contrast theme, and full support for system font scaling up to the largest sizes
- Never rely on colour alone to convey state
- Dark mode by default

### Ads and accessibility — important

Interstitial ads are hostile to screen reader users. Close buttons are frequently unlabelled, countdowns are invisible to TalkBack, and a blind user can be trapped in an ad with no way to find the exit.

This cannot be ignored in an app built for blind readers. Mitigation, in order of preference:

1. **Banner ads rather than interstitials** wherever revenue allows
2. If interstitials are used, place them only **between** readings, never mid-reading
3. Test every ad format with TalkBack before shipping, and drop any format that cannot be reliably dismissed by a screen reader user
4. Consider an inexpensive ad-free tier, with free access granted on request to users of assistive technology

Shipping an inaccessible ad experience in an accessibility app would defeat the entire purpose.

## 4. Card data model

78 cards: 22 Major Arcana, 56 Minor Arcana (Wands, Cups, Swords, Pentacles — Ace through Ten, Page, Knight, Queen, King).

Each card record holds:

| Field | Description |
|---|---|
| `id` | Stable identifier, e.g. `major_19_sun` |
| `name` | "The Sun" |
| `arcana` | Major or Minor |
| `suit` | Wands, Cups, Swords, Pentacles, or none |
| `number` | 0-21 for Majors, 1-14 for Minors |
| `imagery` | **Narrative description of what is depicted.** The most important field in the app. |
| `upright_meaning` | Full interpretation, several sentences |
| `upright_keywords` | 4-6 short keywords |
| `reversed_meaning` | Full interpretation, several sentences |
| `reversed_keywords` | 4-6 short keywords |
| `element` | Fire, Water, Air, Earth, where applicable |
| `art_path` | Image asset reference |
| `art_credit` | Placeholder art or original |

Stored as structured data (JSON) bundled with the app. No network required to do a reading. The app must work fully offline.

## 5. Core mechanics

**Shuffle.** Fisher-Yates on the full 78. Shake-driven, user-controlled duration.

**Reversals.** Each card has an independent chance of being drawn reversed. Default 50%, user-adjustable, and switchable off entirely for readers who do not work with reversals.

**Cut.** Splits the deck at a point determined by the user's swipe.

**Draw.** Cards drawn from the top without replacement within a single reading.

**Spreads for v1:**

- **Single card** — daily draw
- **Three card** — the default. Supports both a labelled mode (past / present / future, situation / action / outcome) and an **open mode with no positional meaning**, for general "what do the cards have for me today" readings
- **Celtic Cross** — ten cards, the classic spread

**Reading journal.** Every reading saved with date, spread, question if given, cards drawn and their orientations, and space for the reader's own notes, enterable by voice. Past readings can be played back as audio.

## 6. Art pipeline

**Placeholder art:** the original 1909 Rider-Waite-Smith illustrations by Pamela Colman Smith are in the public domain in the United States. These are the most recognised tarot images in the world and can be used legally at no cost.

Two cautions:
- Use scans of the genuine 1909 edition. Modern recoloured editions are under copyright.
- Do not use the trademarked deck name in the app title or store listing.

**Original art:** Thomas replaces the placeholders with his own paintings, one card at a time. The app is built so a card's artwork can be swapped by replacing a single asset and updating one field, with no code change.

Realistic scoping: 78 original illustrations is close to a year of painting. The app ships and earns with placeholder art while the original deck is produced. A visible "original art progress" count is worth considering, both as motivation and as proof of work for SAAH donors.

**And a note on the point of it:** because the app describes every card in words, Thomas's own artwork will be described too. A blind reader will be able to experience paintings they cannot see. That is worth doing properly.

## 7. Monetization

Ad-supported and free to download, with revenue directed to SAAH, subject to the accessibility constraints in section 3.

The honest position: ad revenue per user is small, and this app makes money at volume rather than at launch. Its real value to SAAH is twofold — a slow compounding revenue stream, and a piece of visible, meaningful work that tells donors who they are supporting.

## 8. Technical approach

| Layer | Choice |
|---|---|
| App framework | Flutter (single codebase, iOS + Android) |
| Card data | Bundled JSON, fully offline |
| Narration | Platform text-to-speech |
| Shuffle input | Device accelerometer |
| Feedback | Haptics plus audio cues |
| Journal storage | Local device database |
| Ads | Google AdMob, accessibility-constrained |
| Source control | This repository |

## 9. Build order

1. **Repo setup** — repository, README, spec
2. **Card data** — write all 78 records: imagery, meanings, keywords. Large, and it gates everything else.
3. **Deck engine** — shuffle, cut, reversals, draw, spreads
4. **Accessibility foundation** — screen reader labelling and focus order, built in from the first screen rather than retrofitted
5. **Narration** — text-to-speech, playback controls, speech rate
6. **Physical interaction** — shake to shuffle, swipe to cut, tap to draw, with haptics and audio
7. **Reading UI** — card display, spread layouts, high contrast, scalable type
8. **Placeholder art** — public domain imagery integrated
9. **Reading journal** — save, review, voice notes, audio playback
10. **Ad integration** — accessibility-tested, banners preferred
11. **Testing with blind users** — Buddy first. Non-negotiable before shipping.
12. **Store preparation** — icon, screenshots, privacy policy, listings
13. **Ship v1**
14. **Ongoing: replace placeholder art with original paintings, one card at a time**

Step 2 is the long pole on the writing side and step 14 is the long pole on the art side. Neither depends on any code, so both can begin immediately.

## 10. Decisions still needed

- Confirm the app name "Tarot Buddy" is available on both stores
- Whether reversals default on or off
- Whether to include Celtic Cross in v1 or defer to v2
- Whether to offer an ad-free tier, and whether it is free for assistive technology users
- Whose voice for narration: platform TTS only, or optionally a recorded human voice later
- Licence for the repository

---

## Appendix: why this idea is good

The tarot app category is crowded. This one is not competing in it.

Every other tarot app is a picture gallery. This one is built for the readers the pictures were never made available to, and it treats that as the product rather than a compliance checkbox. The market is underserved to the point of being unserved, the technical scope is modest, and the person building it has a reason to care that will outlast the novelty.

It also does something the fundraiser needs: it is a piece of work nobody can mistake for an ask.
