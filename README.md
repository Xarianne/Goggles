# Xariann's Goggles

A small collection of [Brave Search Goggles](https://search.brave.com/goggles) — plain-text rule files that re-rank Brave Search results.

## Goggles

| Goggle | What it does | Raw URL |
| --- | --- | --- |
| [foss-docs](./goggles/foss-docs/goggle.txt) | Boosts official documentation, Arch Wiki, man pages and FOSS project sites; downranks generic listicles and SEO farms. | `https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/foss-docs/goggle.txt` |
| [universal-blue](./goggles/universal-blue/goggle.txt) | Boosts the blogs, docs, forums and project sites of Universal Blue distributions (Bluefin, Aurora, Bazzite, uCore) and the umbrella project. | `https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/universal-blue/goggle.txt` |

Once you have pushed a goggle to GitHub, copy its **Raw** URL (the `raw.githubusercontent.com/...` one, not the `github.com/.../blob/...` one) and submit it at <https://search.brave.com/goggles/create>. Brave fetches, validates and caches the file; you'll get feedback there if anything is malformed.

## Repository layout

```
Goggles/
└── goggles/
    └── <goggle-name>/
        ├── goggle.txt      # the actual goggle (this is the file Brave reads)
        └── README.md       # notes on intent, sources, and tuning history
```

To add a new goggle: create `goggles/<name>/goggle.txt`, fill in the mandatory header (`! name`, `! description`, `! public`, `! author`), and add a row to the table above.

## Updating a goggle

1. Edit the `goggle.txt` file and commit to `main`.
2. Re-submit the goggle's raw URL at <https://search.brave.com/goggles/create>. Brave re-fetches on resubmission — there is no webhook, so this step is required after every change.

Brave does not keep version history of goggle contents; the canonical source is this repo. Tagging releases (`git tag foss-docs-v1`) is a good way to keep your own history.

## Deleting a goggle

Delete the file from this repo, then resubmit the (now-404) URL at <https://search.brave.com/goggles/create> to trigger removal from Brave's cache.

## Syntax cheat sheet

```
! name: ...           # mandatory header
! description: ...
! public: true|false
! author: ...
! homepage: ...       # optional
! issues: ...         # optional
! avatar: #RRGGBB     # optional
! license: ...        # optional

# Patterns match anywhere in the URL by default
/foo/bar

# Anchors: | for start/end of URL
|https://example.org^         # ^ matches a URL delimiter or end-of-URL

# Options after $, comma-separated
$site=archlinux.org
/blog/$site=kernel.org

# Actions
$boost           $boost=3
$downrank        $downrank=2
$discard

# Generic $discard as the first rule = "only keep matched results"
$discard
$boost,site=en.wikipedia.org
```

Precedence when rules conflict: `discard > boost > downrank` (higher boost wins over lower boost).

## Limits (enforced by Brave)

- File size <= 2 MB
- <= 100,000 instructions
- <= 500 chars per instruction
- <= 2 `*` wildcards per instruction
- <= 2 `^` carets per instruction

## References

- [Goggles quickstart](https://github.com/brave/goggles-quickstart) — official syntax walkthrough
- [Getting started](https://github.com/brave/goggles-quickstart/blob/main/getting-started.md) — create / update / delete / share
- [Goggles whitepaper](https://brave.com/goggles) — motivation and design
- [Discover Goggles](https://search.brave.com/goggles/discover) — browse community goggles

## License

The goggle instructions in this repo are released under CC0-1.0 unless a goggle's header says otherwise. See [LICENSE](./LICENSE).