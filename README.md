# YTGuard filter lists

Shared filter lists for [YTGuard](https://github.com/Kruszcontrol/ytguard), the family YouTube controls for Linux + Chrome.
Like ad-block or Pi-hole lists: subscribe in YTGuard under **Filters → Lists**, and they update automatically once a day.

Your own filter entries in YTGuard always beat a list's entry of the same type, so you can override anything a list does
(e.g. allow one channel a list blocks).

## Lists

| List | Ages | What it does |
|---|---|---|
| [Young kids — basics](lists/young-kids-basics.txt) | 0–8 | Hides what YouTube marks not family-safe; parent's OK for live streams, videos over 30 min, news, adult cartoons |
| [Scary and horror](lists/scary-and-horror.txt) | 0–12 | Hides horror, gore, creepypasta; OK needed for spooky games and mascot-horror |
| [Violence and weapons](lists/violence-and-weapons.txt) | 0–12 | Hides graphic violence; OK needed for fights, guns, true crime |
| [Mature and adult content](lists/mature-content.txt) | 0–15 | Hides sexual/adult content; OK needed for alcohol, drugs, swearing |
| [Dangerous challenges and pranks](lists/dangerous-challenges-and-pranks.txt) | 0–15 | Hides dangerous viral challenges; OK needed for pranks |
| [Gambling and scams](lists/gambling-and-scams.txt) | 0–17 | Hides free-Robux/V-Bucks scams; OK needed for gambling and loot boxes |
| [Brainrot and meme spam](lists/brainrot.txt) | 0–12 | Optional: OK needed for Skibidi/brainrot meme content |

**Suggested combinations:** under 9 — all of them; 9–12 — all except *Young kids — basics*; 13–15 — *Mature*, *Dangerous challenges*, *Gambling*.

When you subscribe, you choose what each list does with its entries: **Mixed** (the default — the list's own marking per entry:
clearly inappropriate entries are in `[hide deny]` and never appear, borderline ones are in `[block deny]` and show with a lock so the
kid can ask you), **Block everything**, or **Hide everything**. The "Hides / OK needed" wording above describes Mixed.
The YTGuard app reads [`catalog.json`](catalog.json) to show these as recommended lists.

## List format

Plain text, UTF-8:

```
! Title: Scary and horror
! Description: One or two sentences shown in YTGuard.
! Ages: 0-12
! Homepage: https://github.com/Kruszcontrol/ytguard-lists
! License: CC0-1.0
! Version: 2026-10-02

# comments start with #
[hide deny]                     ← never shown
keyword: creepypasta            ← whole word, in the title
keyword(title,description,tags): jumpscare
keyword(substring): five nights at freddy
keyword(regex): \bf[\*u]+ck
channel: UCxxxxxxxxxxxxxxxxxxxxxx @handle Channel name
channel: @handle Channel name
video: dQw4w9WgXcQ Video title
category: News & Politics
attribute: not_family_safe      ← also: shorts, live, not_made_for_kids, longer_than:30

[block deny]                    ← shown with a lock; kid can ask a parent
keyword: prank

[block allow]                   ← always allowed to play (use sparingly)
```

Sections: `[hide deny]`, `[hide allow]`, `[block deny]`, `[block allow]`.
Keyword options: fields `title` (default), `description`, `tags`, `channel`; match `word` (default, case-insensitive whole words),
`substring` or `regex` (Go/RE2 syntax, case-insensitive).
YTGuard skips lines it doesn't understand and shows them as warnings.

## Contributing

Pull requests welcome. Please:

1. **Mark as `[hide deny]` only what's clearly inappropriate** for every kid in the list's age range, and use `[block deny]` for borderline content.
   (Mixed — the default — uses this marking; parents can switch a list to block or hide everything.)
2. **Avoid false positives.** Prefer whole words and specific phrases; think of innocent titles that would match
   (e.g. "graphic" would catch graphic-design videos, "gang" catches *Gang Beasts*, "sigma" catches maths — use
   "graphic content", "gang violence", "sigma male"). Mention what you checked in the PR.
3. **Channels:** use the channel ID (`UC…`, from the channel page's "Share channel" → "Copy channel ID") plus the @handle and name,
   and explain why in the PR. One-off videos are better reported as a video entry than blocking a whole channel.
4. Bump `! Version:` to today's date.
5. Check the file with YTGuard: `ytguard list-check lists/*.txt` (the GitHub check runs this too).

New lists are welcome too — add the file under `lists/` and an entry in `catalog.json`.

## License

The lists are dedicated to the public domain ([CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/)): anyone may use, copy and adapt them.
