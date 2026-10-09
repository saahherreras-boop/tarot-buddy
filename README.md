# Tarot Buddy

An accessibility-first tarot deck, built so blind and low-vision readers can work with the cards independently.

Named for Buddy, who has been blind since birth.

## Why this exists

Tarot is an intensely visual practice. The whole tradition rests on imagery: a woman with her hands on a lion's jaws, a child on a white horse beneath a blazing sun, a magician with the four suits laid out on his table. Readers learn by looking, and the meanings live in the pictures.

Which has meant tarot is, in practice, close to unavailable to blind readers. Braille decks exist but are rare and expensive. Most tarot apps are picture galleries with text bolted on, and range from awkward to unusable with a screen reader.

Tarot Buddy inverts that. Audio is the primary interface, not an accommodation. Sighted users get a normal tarot app. Blind users get the first one genuinely built for them.

## The core idea

**Describe the picture, not just the meaning.**

A blind reader should not simply be told "The Sun means joy." They should be told what is on the card: the child, the white horse, the banner, the sunflowers, the wall, the blazing sun with a human face. The imagery is the tradition. Withholding it and handing over a keyword list instead is the exact failure this app exists to correct.

## Features

- Full TalkBack and VoiceOver support, tested on real devices
- Built-in narration using platform text-to-speech, with adjustable speech rate
- Narrated imagery for every one of the 78 cards, not just meanings
- Shake the phone to shuffle, swipe to cut, tap to draw, with haptic and audio feedback throughout
- Single card, three card, and Celtic Cross spreads
- Open readings with no positional meaning, as well as labelled spreads
- Reversals, toggleable and adjustable
- Reading journal with voice notes and audio playback
- High contrast, full font scaling, no meaning conveyed by colour alone
- Works entirely offline

## Status

Pre-build. The full specification is in [`SPEC.md`](./SPEC.md). Build order is tracked in Issues.

## Why it is being built

Ad revenue from this app goes to **Starving Artists Against Hunger (SAAH)**, a fundraiser getting food and basic supplies to homeless communities in the Tucson desert.

- Fundraiser: https://gofund.me/922b24d9b

## Art

Placeholder artwork uses the original 1909 Rider-Waite-Smith illustrations by Pamela Colman Smith, which are in the public domain in the United States. These are replaced over time with original paintings by the author.

Because the app describes every card in words, those original paintings will be described too. A blind reader will be able to experience artwork they cannot see.

## Planned stack

| Layer | Choice |
|---|---|
| App framework | Flutter (single codebase, iOS + Android) |
| Card data | Bundled JSON, fully offline |
| Narration | Platform text-to-speech |
| Shuffle input | Device accelerometer |
| Feedback | Haptics plus audio cues |
| Journal storage | Local device database |
| Ads | Google AdMob, accessibility-constrained |

## Author

Thomas Herreras — Tucson, Arizona.

## Licence

To be decided before v1 ships.
