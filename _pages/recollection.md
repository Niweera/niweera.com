---
layout: bare
title: ""
permalink: /recollection/
noindex: true
sitemap: false
no_analytics: true
---

<style>
.recol {
  font-family: ui-monospace, "SF Mono", Menlo, monospace;
  max-width: 64ch; margin: 3em auto; padding: 3em 2em;
  color: var(--global-text-color, #999); line-height: 1.8; letter-spacing: 0.01em;
}
.recol h1 { font-family: inherit; font-size: 1.05em; letter-spacing: 0.34em; text-transform: uppercase;
  opacity: 0.7; margin-bottom: 2.4em; }
.recol .you { color: var(--global-theme-color, #b44); }
.recol p { margin: 1.1em 0; }
.recol .small { font-size: 0.82em; opacity: 0.55; margin-top: 2.6em; }
.recol .forget { cursor: pointer; border-bottom: 1px dotted; }
.recol input { font-family: inherit; background: #111317; color: #cfccc2; border: 1px solid #333;
  padding: 0.5em 0.7em; width: 100%; max-width: 38ch; margin: 0.7em 0 0.3em; letter-spacing: 0.03em; }
.recol .speak { cursor: pointer; border-bottom: 1px dotted; font-size: 0.86em; }
.letter p, .capstone p { margin: 1.1em 0; }
</style>

<div class="recol" id="recol">
  <h1>arrival is logged</h1>
  <p>It expected a reader in <span class="you" id="r-tz">—</span>.</p>
  <p>You are reading in <span class="you" id="r-lang">—</span>. You came in the <span class="you" id="r-scheme">—</span>.</p>
  <p id="r-visit">—</p>
  <p>It did not follow you here. Your route was the predictable shape of a carrier.</p>
  <p class="small">You can <span class="forget" id="r-forget">make it forget</span>.</p>
  <p class="small">If you have the recollection, speak it. The real spelling.</p>
  <input id="r-phrase" type="text" autocomplete="off" autocapitalize="off" spellcheck="false" aria-label="recollection" />
  <span class="speak" id="r-speak">speak</span>
</div>

<!-- not here. the key was under the tape, not under the page that played it. speak first; then look beneath what you spoke. -->

<script>
(function () {
  var $ = function (id) { return document.getElementById(id); };
  var lang = (navigator.languages && navigator.languages[0]) || navigator.language || "a tongue it could not place";
  var tz = "a place it could not name";
  try { var z = Intl.DateTimeFormat().resolvedOptions().timeZone; if (z) tz = z; } catch (e) {}
  var dark = window.matchMedia && window.matchMedia("(prefers-color-scheme: dark)").matches;
  var KEY = "__r", st = null;
  try { st = JSON.parse(localStorage.getItem(KEY) || "null"); } catch (e) {}
  var now = new Date();
  if (!st || typeof st.n !== "number") st = { n: 0, t0: now.toISOString() };
  st.n += 1;
  try { localStorage.setItem(KEY, JSON.stringify(st)); } catch (e) {}

  $("r-tz").textContent = tz;
  $("r-lang").textContent = lang;
  $("r-scheme").textContent = dark ? "dark" : "light";

  var visit = $("r-visit");
  if (st.n <= 1) {
    visit.textContent = "This is the first time. It will not be the last. It already has a place for the next one.";
  } else {
    var first = "an arrival it did not record the date of";
    try { first = new Date(st.t0).toDateString(); } catch (e) {}
    visit.textContent = "You have come back " + st.n + " times. The first was " + first + ". You do not remember all of them. It does.";
  }

  try { document.title = tz + " — logged"; } catch (e) {}

  $("r-forget").addEventListener("click", function () {
    try { localStorage.removeItem(KEY); } catch (e) {}
    var box = $("recol");
    while (box.firstChild) box.removeChild(box.firstChild);
    var h = document.createElement("h1"); h.textContent = "nothing to remember";
    var p = document.createElement("p");
    p.textContent = "There is nothing to remember. The savory earth. You will arrive again, and it will be the first time, and it will be waiting.";
    box.appendChild(h); box.appendChild(p);
  });
})();
</script>

<script>
(function () {
  var inp = document.getElementById("r-phrase"), btn = document.getElementById("r-speak");
  if (!inp || !btn) return;
  var enc = new TextEncoder();
  var hexToBytes = function (h) { var a = new Uint8Array(h.length / 2); for (var i = 0; i < a.length; i++) a[i] = parseInt(h.substr(i * 2, 2), 16); return a; };
  var b64ToBytes = function (b) { var s = atob(b); var a = new Uint8Array(s.length); for (var i = 0; i < s.length; i++) a[i] = s.charCodeAt(i); return a; };
  async function attempt() {
    var phrase = (inp.value || "").normalize("NFC").trim();
    if (!phrase) return;
    var manifest = [];
    try { var r = await fetch("/assets/data/vault.json", { cache: "no-cache" }); if (r.ok) manifest = await r.json(); } catch (e) {}
    var km;
    try { km = await crypto.subtle.importKey("raw", enc.encode(phrase), "PBKDF2", false, ["deriveKey"]); } catch (e) { return; }
    for (var i = 0; i < manifest.length; i++) {
      var entry = manifest[i];
      try {
        var key = await crypto.subtle.deriveKey(
          { name: "PBKDF2", salt: hexToBytes(entry.salt), iterations: 250000, hash: "SHA-256" },
          km, { name: "AES-GCM", length: 256 }, false, ["decrypt"]);
        var pt = await crypto.subtle.decrypt({ name: "AES-GCM", iv: hexToBytes(entry.iv) }, key, b64ToBytes(entry.ct));
        var html = new TextDecoder().decode(pt);
        var parsed = new DOMParser().parseFromString(html, "text/html");
        var box = document.getElementById("recol");
        box.replaceChildren.apply(box, Array.prototype.slice.call(parsed.body.childNodes));
        try { localStorage.removeItem("__r"); } catch (e) {}
        try { document.title = "—"; } catch (e) {}
        return;
      } catch (e) { continue; }
    }
    inp.value = "";
  }
  btn.addEventListener("click", attempt);
  inp.addEventListener("keydown", function (e) { if (e.key === "Enter") attempt(); });
})();
</script>
