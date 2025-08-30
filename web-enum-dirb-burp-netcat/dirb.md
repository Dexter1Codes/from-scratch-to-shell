### Before we start Dirb, I check robots.txt first:
```bash
curl http://127.0.0.1/DVWA/robots.txt
```
and the output was:
` User-agent: *
Disallow: /
`
So basically, the site is telling bots not to crawl anything. Not a real security mechanism but just a hint.
To me, it was like a little flag saying:
"okay, maybe there's something deep worth hiding here"
Good enough for me to dig deeper.

## DIRB
I firstly ran basic Dirb scan like:
```bash
dirb http://127.0.0.1/DVWA
```
It started listing out paths with status codes like:
- 200 OK: File/dir is accessible
- 301/302: Redirects (could still be interesting)
to further check for some files or dirs, i used:
```bash
dirb http://127.0.0.1/DVWA