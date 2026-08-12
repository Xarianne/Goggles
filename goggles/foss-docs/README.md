# foss-docs

A Brave Search Goggle that shows only authoritative documentation, distro wikis, man pages, upstream project sites, and package registries for Linux and FOSS queries.

## Intent

When you search something like `systemd timer vs cron` or `btrfs scrub`, Brave's default ranking surfaces a mix of official docs and SEO-optimised listicles. This goggle:

- **Restricts** results to official documentation, distro wikis (Arch, Debian, Fedora, Gentoo, NixOS, openSUSE, Alpine, Ubuntu), man pages (man7.org, linux.die.net), the Linux kernel docs, the TLDP, upstream project sites (GNOME, KDE, systemd.io, nginx, PostgreSQL, Redis, Ansible, Vim/Neovim/Emacs), language docs (Python, Rust, Go, PHP), and package registries (Arch packages, Debian/Ubuntu packages, Repology, PyPI, crates.io, pkg.go.dev, npm, Flathub).
- **Hides** everything else. It is not a boost goggle.

## Sources used to build this goggle

- Each distribution's official documentation domain.
- Each upstream project's official docs or source repository site.
- Common package registry and documentation hub domains.

## Tuning notes

Goggles need iteration. Process:

1. Search a FOSS query in Brave with this goggle active.
2. Brave shows a per-result explanation of which rule fired — click the goggle icon / "Why am I seeing this" UI.
3. If a bad result is too high or a good result is missing, add a rule here.
4. Commit and re-submit the raw URL at <https://search.brave.com/goggles/create>. There's no webhook — Brave re-fetches only on manual resubmission.
5. Repeat.

### Things to watch for

- **Over-boosting**: too many `$boost,site=...` rules with equal strength can wash each other out. Use `$boost=2` or `$boost=3` to give canonical sources (kernel.org, man7.org, wiki.archlinux.org) priority over the rest.
- **Subdomain matches**: `$site=archlinux.org` also matches `wiki.archlinux.org` and `archlinux.org/packages`.
- **Missing results**: this goggle discards anything not matching a rule. If a useful source is being removed, add it here.

## Raw URL

```
https://raw.githubusercontent.com/Xarianne/Goggles/main/goggles/foss-docs/goggle.txt
```

Submit this at <https://search.brave.com/goggles/create> to activate. Re-submit after every edit.

## License

CC0-1.0. See the repo's [LICENSE](../../LICENSE).
