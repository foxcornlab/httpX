<h1 align="center">
  <img src="static/httpx-logo.png" alt="httpX" width="200px">
  <br>
</h1>

<h4 align="center">fast + multipurpose `http` toolkit for `recon` and `probing`</h4>

<p align="center">
  <a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-_red.svg"></a>
  <a href="https://github.com/foxcornlab/httpX/releases"><img src="https://img.shields.io/github/release/foxcornlab/httpX"></a>
</p>

<p align="center">
  <a href="#what">What</a> •
  <a href="#probes">Probes</a> •
  <a href="#install">Install</a> •
  <a href="#usage">Usage</a> •
  <a href="#notes">Notes</a> •
  <a href="#credits">Credits</a>
</p>

---

# `what`

`httpX` is a `Foxcorn Lab` build of [`httpx`](https://github.com/projectdiscovery/httpx) (`v1.12.0`, core byte-identical) by `ProjectDiscovery`. It runs multiple HTTP `probes` on top of the `retryablehttp` library and is built to keep result `reliability` high while you crank `threads`. A staple in every recon `pipeline`.

> Straight up: this is a maintained `fork`, not a new tool. All the real work is upstream's. This build keeps the code working under the `Foxcorn Lab` banner, rebrands the binary, strips the junk from my old tree, and ships it with a sane default `user-agent`.

# `probes`

* `http` probing — status codes, content length, `titles`, final redirects
* `tech` detection via `wappalyzergo` — what the target is built with
* `cdn` check — is it sitting behind a `cdn` / `waf`
* `jarm` + `favicon` (`mmh3`) + `body` hashes — fingerprinting without banners
* `headless` mode — real `chromium` rendering + `screenshots`
* `dsl` filtering — slice results with a filter expression language
* `asn` / `cdn` / `tls` data enrichment on every probe
* `api` server endpoint — feed results into your own tooling
* `resume` support — pick up interrupted runs where they died
* `csp` / `vhost` / `websocket` probes

# `install`

```bash
# from source
git clone https://github.com/foxcornlab/httpX
cd httpX
go build -o httpX ./cmd/httpx

# docker
docker build -t foxcornlab/httpX .
docker run foxcornlab/httpX -l targets.txt -json
```

Needs `go >= 1.21`. Headless/screenshot features need `chromium` on the box.

# `usage`

```bash
# basic probe
echo https://example.com | ./httpX

# recon pipeline style
cat targets.txt | ./httpX -title -tech-detect -status-code -json -o probed.json

# filter with dsl, keep it quiet
cat targets.txt | ./httpX -silent -dsl 'status_code==200 && contains(content_type, "text/html")'

# headless screenshots of live hosts
cat targets.txt | ./httpX -headless -screenshot -o screenshots/
```

Full flag list: `./httpX -h`. Upstream docs at https://docs.projectdiscovery.io/tools/httpx/ still apply one-to-one.

# `notes`

* This is a `fork`. Upstream bug? Check [`projectdiscovery/httpx`](https://github.com/projectdiscovery/httpx) issues first — the code here is theirs.
* Don't be a menace: `threads` are cheap, other people's bandwidth isn't. Keep `-rate-limit` sane and stay inside your scope.
* `educational` use only. Probe what you own or what you're authorised to test. Anything else is on you.

# `credits`

```
Thanks to
  - ProjectDiscovery team — the actual authors of httpx
  - every contributor on the upstream repo
```

`Licence`: `MIT` (upstream, kept intact — see `LICENSE.md`).

> Research by `Vineeth Kumar` (`foxcornlab`). Found a bug or want a feature, open an `issue`.
