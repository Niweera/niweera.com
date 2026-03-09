---
layout: bare
title: ""
permalink: /registry/
noindex: true
sitemap: false
no_analytics: true
---

<style>
.rg { font-family: ui-monospace, "SF Mono", Menlo, monospace; max-width: 76ch; margin: 2.6em auto; padding: 2.6em 2em; color: #cfccc2; line-height: 1.7; }
.rg h1 { font-size: 1.05em; letter-spacing: 0.3em; text-transform: uppercase; opacity: 0.7; }
.rg h2 { font-size: 0.9em; letter-spacing: 0.18em; text-transform: uppercase; opacity: 0.55; margin: 2.4em 0 0.6em; }
.rg small { opacity: 0.55; }
.rg .roll { background: #0d0f12; border: 1px solid #2a2d33; padding: 1.2em 1.3em; overflow-x: auto; font-size: 0.84em; color: #aeb4ba; white-space: pre; }
.rg .k { color: #b9a36a; }
</style>

<div class="rg">

<h1>the roll of the re-lit</h1>

<p>This is the record. It is append-only: a name is added, never removed, and every line is bound to the line before it, so the order cannot be changed without breaking the chain. The whole of it is signed by the keeper.</p>

<p>Everywhere else, this site forgets you the moment you leave. Here it does not. That is the arrangement — you are forgotten where you stood, and kept where you cannot watch.</p>

<pre class="roll">-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

THE ROLL OF THE RE-LIT

Append-only. Each line is hash-bound to the line before it; the whole is signed by the keeper.
The site forgets you. This does not.

  no.  remembered as              read at (UTC)         chain
  000  — the watch opens —     2026-05-30T00:00:00Z  787c5876ef70929e
-----BEGIN PGP SIGNATURE-----

wr0EARYKAG8Fgmobgi0JEC0mK4cSAGGNRxQAAAAAAB4AIHNhbHRAbm90YXRpb25z
LnNlcXVvaWEtcGdwLm9yZ5vmFJE9v6sbEDDETvi7MfovuHWVu2SmyS7ZcR8VteBl
FiEEOdjIOFIaZ/rZL23aLSYrhxIAYY0AACWcAP9Z/7txXr29ybEUaeRjnJKsfwFh
nFKHGHc1FJEnd9kzngEA79IK4rwjmZjxQHTbOfIKAfv97Xa6Lmew7cAArTi+Xgs=
=dZOY
-----END PGP SIGNATURE-----</pre>

<h2>how a name is added</h2>

<p>There is no button here, and no claim. The record does not trust a machine — because anything a machine can check, a stranger who was merely <em>told</em> the answer can feed it just as well. So a machine checks nothing.</p>

<p>A name is added by hand. When you hold the words — the ones you were asked to remember, said correctly, to the last mark — you seal them, together with one line of your own, to the keeper's key, and you send them the way the letter taught you. The keeper reads it. If it is true, you become the next line in the watch. There is no deadline.</p>

<h2>verify</h2>

<p>Take nothing here on faith. Copy the block above and run <span class="k">gpg --verify</span> against the keeper's key <small>39D8 C838 521A 67FA D92F 6DDA 2D26 2B87 1200 618D</small>. The last column of each row is its place in the chain — <small>sha256( no. | remembered-as | time | chain-of-the-line-before )</small>. Recompute it down the whole record, and you will know whether a single line has been touched between you and the first.</p>

<p><small>the site forgets you. this does not.</small></p>

</div>
