# foss-docs

A Brave Search Goggle that re-ranks Linux / FOSS queries toward authoritative primary sources.

## Intent

When you search something like `systemd timer vs cron` or `btrfs scrub`, Brave's default ranking surfaces a mix of official docs and SEO-optimised listicles. This goggle:

- **Boosts** official documentation, distro wikis (Arch, Debian, Fedora, Gentoo, NixOS, openSUSE, Alpine, Ubuntu), man pages (man7.org, linux.die.net), the Linux kernel docs, the TLDP, upstream project sites (GNOME, KDE, systemd.io, nginx, PostgreSQL, Redis, Ansible, Vim/Neovim/Emacs), language docs (Python, Rust, Go, PHP), and package registries (Arch packages, Debian/Ubuntu packages, Repology, PyPI, crates.io, pkg.go.dev, npm, Flathub).
- **Downranks** listicle / SEO farms (HowToGeek, MakeUseOf, Lifewire, GeeksforGeeks, TutorialsPoint, Medium, dev.to, LinuxConfig, It's FOSS, etc.) — these aren't removed, just pushed down so primary sources win.
- **Discards** AI-generated content mills that copy Stack Overflow / GitHub content (copyprogramming.com, faqcode.com, coderecipe.org, etc.).

## Tuning notes

Goggles need iteration. Process:

1. Search a FOSS query in Brave with this goggle active.
2. Brave shows a per-result explanation of which rule fired — click the goggle icon / "Why am I seeing this" UI.
3. If a bad result is too high or a good result is missing, add a rule here.
4. Commit and re-submit the raw URL at <https://search.brave.com/goggles/create>. There's no webhook — Brave re-fetches only on manual resubmission.
5. Repeat.

### Things to watch for

- **Over-boosting**: too many `$boost,site=...` rules with equal strength can wash each other out. Use `$boost=2` or `$boost=3` to give canonical sources (kernel.org, man7.org, wiki.archlinux.org) priority over the rest.
- **Downrank vs discard**: prefer `$downrank` over `$discard` for sites that occasionally have good content (Medium, dev.to). Reserve `$discard` for pure content farms.
- **Subdomain matches**: `$site=archlinux.org` also matches `wiki.archlinux.org` and `archlinux.org/packages`. You don't need a separate rule unless you want a different strength.
- **Conflict precedence**: `discard > boost > downrank`. A `$discard` on a more specific path wins over a `$boost` on the site.

## Sources for the boost/discard lists

- Distributions and upstream projects: from each project's official documentation domain.
- Content-farm discard list: adapted from community blocklists (e.g. the "Copycats removal" goggle in `brave/goggles-quickstart`) plus manual additions.
- Listicle downrank list: based on queries like `linux btrfs snapshot`, `nginx reverse proxy`, `iptables vs nftables` and observing which non-authoritative sites rank high.

## Raw URL

```
https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/foss-docs/goggle.txt
```

Submit this at <https://search.brave.com/goggles/create> to activate. Re-submit after every edit.

## License

CC0-1.0. See the repo's [LICENSE](../../LICENSE).