# universal-blue

A Brave Search Goggle that boosts the blogs, docs, forums and project sites of [Universal Blue](https://universal-blue.org/) distributions: Bluefin (and Dakota, LTS, Server), Aurora, Bazzite, and uCore.

## Intent

When you search something like `bluefin rebase to aurora`, `bazzite game mode`, `rpm-ostree rollback`, or `ublue akmods`, Brave's default ranking mixes official UB content with generic Linux blog coverage of varying depth. This goggle:

- **Boosts** the umbrella site and Discourse forums (`universal-blue.org`, `universal-blue.discourse.group`), each distro's site and docs (`projectbluefin.io`, `getaurora.dev`, `docs.getaurora.dev`, `bazzite.gg`, `projectucore.io`), the Bluefin factory dashboard, and the two GitHub orgs (`ublue-os`, `projectbluefin`).
- Leaves unmatched results alone — this is a **boost-only** goggle, not a restrict. Generic coverage still appears, just below the official sources.

## Sources used to build this goggle

- `https://universal-blue.org/` — lists the four distros and their sites, and links the Discourse + YouTube.
- The distro blog indexes: `https://docs.projectbluefin.io/blog/` and `https://docs.getaurora.dev/blog`.
- `https://universal-blue.discourse.group/` — Discourse hosts Bazzite news (tag `bazzite-news`), announcements (`announcements`), and the docs forum.
- Bazzite has no separate blog — news goes through Discourse. uCore's site (`projectucore.io`) is mostly GitHub-hosted docs.

## YouTube — and its limitation

Universal Blue's video presence is Jorge Castro's channel: `https://www.youtube.com/c/JorgeCastro` (also reachable as `youtube.com/@JorgeCastro`). It is included as a boost rule.

**Important caveat:** Goggles match on URL substrings, and YouTube video URLs are `youtube.com/watch?v=XXXXX` — the channel name does **not** appear in the URL. This means:

- The channel **page itself** (`youtube.com/@JorgeCastro`) is boosted. ✓
- Individual **videos** from the channel are **not** boosted, because the rule can't see which channel a `watch?v=` URL belongs to. ✗

There is no clean fix within the Goggle language — Brave does not expose a `channel=` option. Workarounds:

1. **Manual:** pin specific video IDs you care about as explicit `/watch?v=XXXXX` patterns. Brittle, and new videos won't be covered.
2. **Accept it:** the channel page boost is enough when it appears in results; for everything else rely on the textual match of "bluefin"/"aurora"/"bazzite" in titles and descriptions to rank the videos naturally.
3. **Use YouTube's in-site search** (`site:youtube.com bluefin`) and skip the goggle for video queries — the channel's own upload page is the real index anyway.

This goggle takes option 2: boost the channel page, document the limitation, don't pretend it does what it can't.

## Tuning notes

Goggles need iteration. Process:

1. Search a UB query in Brave with this goggle active.
2. Brave shows a per-result explanation of which rule fired — click the goggle icon / "Why am I seeing this" UI.
3. If a wrong result is too high or a good result is missing, add a rule here.
4. Commit and re-submit the raw URL at <https://search.brave.com/goggles/create>. There is no webhook — Brave re-fetches only on manual resubmission.
5. Repeat.

### Things to watch for

- **Boost strength:** the umbrella sites get `$boost=3`, GitHub orgs and the factory dashboard get `$boost=2`. If GitHub issue threads are drowning out the actual blog posts, drop the GitHub rule to `$boost=1` or add `/discussions$boost=3,site=github.com` to favor discussions over issues.
- **Subdomain matches:** `$site=projectbluefin.io` also matches `docs.projectbluefin.io` and `factory.projectbluefin.io`. They have separate rules only because they deserve different strengths (docs = 3, factory = 2).
- **Adding a "strict mode":** if you want this goggle to *only* show UB sites and hide everything else, add a generic `$discard` as the first instruction after the header, so unmatched results get removed:

  ```
  $discard
  $boost=3,site=universal-blue.org
  ... (rest of rules)
  ```
  This is aggressive and not the default — the boost-only default keeps the goggle useful for general Linux queries that touch UB topics.

## Raw URL

```
https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/universal-blue/goggle.txt
```

Submit this at <https://search.brave.com/goggles/create> to activate. Re-submit after every edit.

## License

CC0-1.0. See the repo's [LICENSE](../../LICENSE).