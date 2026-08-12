# universal-blue

A Brave Search Goggle that shows only results from [Universal Blue](https://universal-blue.org/) and its distributions: Bluefin (and Dakota, LTS, Server), Aurora, Bazzite, and uCore.

## Intent

When you search something like `bluefin rebase to aurora`, `bazzite game mode`, `rpm-ostree rollback`, or `ublue akmods`, Brave's default ranking mixes official UB content with generic Linux blog coverage of varying depth. This goggle:

- **Restricts** results to the umbrella site and Discourse forums (`universal-blue.org`, `universal-blue.discourse.group`), each distro's site and docs (`projectbluefin.io`, `getaurora.dev`, `docs.getaurora.dev`, `bazzite.gg`, `projectucore.io`), the Bluefin factory dashboard, and the two GitHub orgs (`ublue-os`, `projectbluefin`).
- **Hides** unmatched results. It is not a boost goggle.

## Sources used to build this goggle

- `https://universal-blue.org/` — lists the four distros and their sites, and links the Discourse.
- The distro blog indexes: `https://docs.projectbluefin.io/blog/` and `https://docs.getaurora.dev/blog`.
- `https://universal-blue.discourse.group/` — Discourse hosts Bazzite news (tag `bazzite-news`), announcements (`announcements`), and the docs forum.
- Bazzite has no separate blog — news goes through Discourse. uCore's site (`projectucore.io`) is mostly GitHub-hosted docs.

## Tuning notes

Goggles need iteration. Process:

1. Search a UB query in Brave with this goggle active.
2. Brave shows a per-result explanation of which rule fired — click the goggle icon / "Why am I seeing this" UI.
3. If a wrong result is too high or a good result is missing, add a rule here.
4. Commit and re-submit the raw URL at <https://search.brave.com/goggles/create>. There is no webhook — Brave re-fetches only on manual resubmission.
5. Repeat.

### Things to watch for

- **Boost strength:** the umbrella sites get `$boost=3`, GitHub orgs and the factory dashboard get `$boost=2`. If GitHub issue threads are drowning out the actual blog posts, drop the GitHub rule to `$boost=1` or add `/discussions^$boost=3,site=github.com` to favor discussions over issues.
- **Subdomain matches:** `$site=projectbluefin.io` also matches `docs.projectbluefin.io` and `factory.projectbluefin.io`. They have separate rules only because they deserve different strengths (docs = 3, factory = 2).
- **Missing results:** this goggle discards anything not matching a rule. If a useful third-party source is being removed, either add it or use the `foss-docs` goggle instead.

## Raw URL

```
https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/universal-blue/goggle.txt
```

Submit this at <https://search.brave.com/goggles/create> to activate. Re-submit after every edit.

## License

CC0-1.0. See the repo's [LICENSE](../../LICENSE).
