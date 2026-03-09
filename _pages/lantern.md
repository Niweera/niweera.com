---
layout: lantern
title: ""
permalink: /lantern/
noindex: true
sitemap: false
no_analytics: true
---

<style>
.ln { font-family: ui-monospace, "SF Mono", Menlo, monospace; max-width: 64ch; margin: 3em auto; padding: 2.4em 1.8em; line-height: 1.7; }
.ln h1 { font-size: 1em; letter-spacing: 0.42em; text-transform: uppercase; opacity: 0.66; font-weight: 400; }
.ln .warn { border: 1px solid #3a2020; background: #120d0d; color: #b59a9a; padding: 1em 1.1em; margin: 1.6em 0; font-size: 0.86em; }
.ln .grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 0.5em 0.9em; margin: 1.8em 0 0.8em; }
.ln .slot { display: flex; align-items: baseline; gap: 0.5em; }
.ln .slot .n { opacity: 0.45; font-size: 0.72em; min-width: 2.2em; text-align: right; letter-spacing: 0.1em; }
.ln input { font-family: inherit; background: #0d0f13; color: #cfccc2; border: 1px solid #262a31; border-radius: 2px; padding: 0.4em 0.5em; width: 100%; }
.ln input:focus { outline: none; border-color: #4a5260; }
.ln input.lit { border-color: #6a5a2a; color: #d8c98a; }
.ln .bar { display: flex; align-items: center; justify-content: space-between; margin: 1.2em 0; }
.ln .count { opacity: 0.5; font-size: 0.8em; letter-spacing: 0.12em; }
.ln .light { cursor: pointer; border: 1px solid #3a3a45; background: #15171c; color: #cfccc2; padding: 0.55em 1.4em; letter-spacing: 0.22em; text-transform: uppercase; font-family: inherit; font-size: 0.8em; }
.ln .light:hover { border-color: #6a5a2a; color: #d8c98a; }
.ln .dark { opacity: 0.5; font-style: italic; margin: 1em 0; min-height: 1.3em; }
.ln .reveal { border-top: 1px solid #2a2a30; margin-top: 2.2em; padding-top: 1.6em; }
.ln small { opacity: 0.5; }
</style>

<div class="ln">

<h1>the lantern</h1>

<div class="warn">
TWELVE WORDS. You did not choose them — you found them, lying in prose that was not addressed to you, and you
carried them here. Note what this is: everywhere else on the live web, a screen that asks you to speak the
twelve words you carry is the last screen you will read; the words go out on the wire and do not come back,
and neither, in the end, do you. This one is built wrong on purpose. Nothing leaves the tab — do not take
that on faith: open the panel that watches the wire and confirm the silence yourself. The next one will look
like this and will say nothing — and the twelve it asks for will be yours, not these. These you found; yours
you would be giving away. Speak the found words; never the others, never anywhere, never again.
</div>

<div class="grid" id="ln-grid"></div>

<div class="bar">
  <span class="count" id="ln-count">the lantern waits</span>
  <span class="light" id="ln-light">light it</span>
</div>

<p class="dark" id="ln-dark"></p>
<div id="ln-reveal"></div>

<p><small>the words are read in the order you found them, first to last. a wrong word, a wrong order, and the vessel stays dark — it tells you nothing more.</small></p>

</div>

<script>
(function () {
  var ROMAN = ["I","II","III","IV","V","VI","VII","VIII","IX","X","XI","XII"];
  var grid = document.getElementById("ln-grid");
  var inputs = [];
  for (var i = 0; i < 12; i++) {
    var slot = document.createElement("div"); slot.className = "slot";
    var n = document.createElement("span"); n.className = "n"; n.textContent = ROMAN[i];
    var inp = document.createElement("input");
    inp.type = "text"; inp.autocomplete = "off"; inp.spellcheck = false;
    inp.setAttribute("aria-label", "word " + ROMAN[i]);
    slot.appendChild(n); slot.appendChild(inp); grid.appendChild(slot);
    inputs.push(inp);
  }
  var countEl = document.getElementById("ln-count");
  var darkEl = document.getElementById("ln-dark");
  function refresh() {
    var lit = 0;
    for (var i = 0; i < 12; i++) {
      var has = inputs[i].value.trim().length > 0;
      inputs[i].classList.toggle("lit", has);
      if (has) lit++;
    }
    countEl.textContent = (lit === 0 ? "the lantern waits" : lit === 12 ? "twelve carried — light it" : lit + " of twelve carried");
  }
  inputs.forEach(function (inp, idx) {
    inp.addEventListener("input", refresh);
    inp.addEventListener("keydown", function (e) {
      if (e.key === "Enter") { if (idx < 11) inputs[idx + 1].focus(); else light(); }
    });
  });

  var BLOB = {"salt": "7ef1cace0b9232b84c6dd00f3766bd66", "iv": "a6f4015c2a4e83c205e70217", "ct": "KdB7xrZSuMgwMvNwlLgO9RZ+OWLJab2VfU4zD/owuKgRChNua4aDRTp0BH0JW2Q/EH2n3EgHn12eLcHHYkNcGLyCHxviYwhjk628nS6x6De4bP2fNIao7WaMTb2eSlz3nEbODNLaBDtyGHQii2YJ3j3/+p5P1XOcsxuYeSXEZerNGHyj04SpV2MqyA/EqaC7dOUkTzr6AD+tdUjXI/3pD8c+FMAQXiKBLkR6/EqY51G8MkW0wCrU+nM+EhbMQTG2T8oHKJlQImVrjAoV7QeB9EnU7ZOuHrzDLXZamdy9tVY+/WZ7uN0B6O14Gr6d5CtJ4dGHj/JpdFL5tP4t7dMkC4LmyTEWgxNbzR4TWxl931f/atjmv2GzXQNQ0KMbOuCtXRWnRL0u6RCIhUD+Dm5IeDZrXyzp36e8SMzJ0eJQWnf+68cFR91I98she7HfJzkKaxZslTh7sqeTTq845lPT2J1IAJmoeRPqgSz6UJqvDvK8ztMRVs7qRDagfJctZChMvamMiIOiyHZwVfYXOueCFqvGb415kRaCy7CMGQ9Xq3YWaIozzKZGuSZTfayxXiK5WR4en3kuUgVr94daj2U1wBXDPiF6YQwhk+5npRKvAxxwhs7xsgoNQPJXNolHMZTmmL0JOEUPL12wOEiIxVpu3vmE1asCHHnQ5VAhj6xxGeatBJMw8x9t83QqfQCEJ0DT+228QVsy+gRXfhcDRgzxzptp1lbMBRxrNbLxtmzbSlm/U0Wn68ag7nzupYetS39j201V3AYEtEMS6408AJD+7DBmMx0RaRorkHbLB1GEzdct9s0C1vea8fgCRrz7st97ZwnbvwtTgz1CZivrQn2P/C481Pvn0w5m9W87pOS64FE2YqicIT5KupWVIfmEcGx2qzoTtgaRrMKwS5hHGFq7dXThTR+8tqTKBF69DOFOfQvzBv3jOXcZKJq/HHMTa0iCCDeUpYrw6b/lbaEt/vdt6864GD4aNfrQjjc8QTdfJC429mW9Vnm0bor1q3AE9NBwKSXKEQ47v8BsyEP8"};
  var enc = new TextEncoder();
  var WL = "abandon ability able about above absent absorb abstract absurd abuse access accident account accuse achieve acid acoustic acquire across act action actor actress actual adapt add addict address adjust admit adult advance advice aerobic affair afford afraid again age agent agree ahead aim air airport aisle alarm album alcohol alert alien all alley allow almost alone alpha already also alter always amateur amazing among amount amused analyst anchor ancient anger angle angry animal ankle announce annual another answer antenna antique anxiety any apart apology appear apple approve april arch arctic area arena argue arm armed armor army around arrange arrest arrive arrow art artefact artist artwork ask aspect assault asset assist assume asthma athlete atom attack attend attitude attract auction audit august aunt author auto autumn average avocado avoid awake aware away awesome awful awkward axis baby bachelor bacon badge bag balance balcony ball bamboo banana banner bar barely bargain barrel base basic basket battle beach bean beauty because become beef before begin behave behind believe below belt bench benefit best betray better between beyond bicycle bid bike bind biology bird birth bitter black blade blame blanket blast bleak bless blind blood blossom blouse blue blur blush board boat body boil bomb bone bonus book boost border boring borrow boss bottom bounce box boy bracket brain brand brass brave bread breeze brick bridge brief bright bring brisk broccoli broken bronze broom brother brown brush bubble buddy budget buffalo build bulb bulk bullet bundle bunker burden burger burst bus business busy butter buyer buzz cabbage cabin cable cactus cage cake call calm camera camp can canal cancel candy cannon canoe canvas canyon capable capital captain car carbon card cargo carpet carry cart case cash casino castle casual cat catalog catch category cattle caught cause caution cave ceiling celery cement census century cereal certain chair chalk champion change chaos chapter charge chase chat cheap check cheese chef cherry chest chicken chief child chimney choice choose chronic chuckle chunk churn cigar cinnamon circle citizen city civil claim clap clarify claw clay clean clerk clever click client cliff climb clinic clip clock clog close cloth cloud clown club clump cluster clutch coach coast coconut code coffee coil coin collect color column combine come comfort comic common company concert conduct confirm congress connect consider control convince cook cool copper copy coral core corn correct cost cotton couch country couple course cousin cover coyote crack cradle craft cram crane crash crater crawl crazy cream credit creek crew cricket crime crisp critic crop cross crouch crowd crucial cruel cruise crumble crunch crush cry crystal cube culture cup cupboard curious current curtain curve cushion custom cute cycle dad damage damp dance danger daring dash daughter dawn day deal debate debris decade december decide decline decorate decrease deer defense define defy degree delay deliver demand demise denial dentist deny depart depend deposit depth deputy derive describe desert design desk despair destroy detail detect develop device devote diagram dial diamond diary dice diesel diet differ digital dignity dilemma dinner dinosaur direct dirt disagree discover disease dish dismiss disorder display distance divert divide divorce dizzy doctor document dog doll dolphin domain donate donkey donor door dose double dove draft dragon drama drastic draw dream dress drift drill drink drip drive drop drum dry duck dumb dune during dust dutch duty dwarf dynamic eager eagle early earn earth easily east easy echo ecology economy edge edit educate effort egg eight either elbow elder electric elegant element elephant elevator elite else embark embody embrace emerge emotion employ empower empty enable enact end endless endorse enemy energy enforce engage engine enhance enjoy enlist enough enrich enroll ensure enter entire entry envelope episode equal equip era erase erode erosion error erupt escape essay essence estate eternal ethics evidence evil evoke evolve exact example excess exchange excite exclude excuse execute exercise exhaust exhibit exile exist exit exotic expand expect expire explain expose express extend extra eye eyebrow fabric face faculty fade faint faith fall false fame family famous fan fancy fantasy farm fashion fat fatal father fatigue fault favorite feature february federal fee feed feel female fence festival fetch fever few fiber fiction field figure file film filter final find fine finger finish fire firm first fiscal fish fit fitness fix flag flame flash flat flavor flee flight flip float flock floor flower fluid flush fly foam focus fog foil fold follow food foot force forest forget fork fortune forum forward fossil foster found fox fragile frame frequent fresh friend fringe frog front frost frown frozen fruit fuel fun funny furnace fury future gadget gain galaxy gallery game gap garage garbage garden garlic garment gas gasp gate gather gauge gaze general genius genre gentle genuine gesture ghost giant gift giggle ginger giraffe girl give glad glance glare glass glide glimpse globe gloom glory glove glow glue goat goddess gold good goose gorilla gospel gossip govern gown grab grace grain grant grape grass gravity great green grid grief grit grocery group grow grunt guard guess guide guilt guitar gun gym habit hair half hammer hamster hand happy harbor hard harsh harvest hat have hawk hazard head health heart heavy hedgehog height hello helmet help hen hero hidden high hill hint hip hire history hobby hockey hold hole holiday hollow home honey hood hope horn horror horse hospital host hotel hour hover hub huge human humble humor hundred hungry hunt hurdle hurry hurt husband hybrid ice icon idea identify idle ignore ill illegal illness image imitate immense immune impact impose improve impulse inch include income increase index indicate indoor industry infant inflict inform inhale inherit initial inject injury inmate inner innocent input inquiry insane insect inside inspire install intact interest into invest invite involve iron island isolate issue item ivory jacket jaguar jar jazz jealous jeans jelly jewel job join joke journey joy judge juice jump jungle junior junk just kangaroo keen keep ketchup key kick kid kidney kind kingdom kiss kit kitchen kite kitten kiwi knee knife knock know lab label labor ladder lady lake lamp language laptop large later latin laugh laundry lava law lawn lawsuit layer lazy leader leaf learn leave lecture left leg legal legend leisure lemon lend length lens leopard lesson letter level liar liberty library license life lift light like limb limit link lion liquid list little live lizard load loan lobster local lock logic lonely long loop lottery loud lounge love loyal lucky luggage lumber lunar lunch luxury lyrics machine mad magic magnet maid mail main major make mammal man manage mandate mango mansion manual maple marble march margin marine market marriage mask mass master match material math matrix matter maximum maze meadow mean measure meat mechanic medal media melody melt member memory mention menu mercy merge merit merry mesh message metal method middle midnight milk million mimic mind minimum minor minute miracle mirror misery miss mistake mix mixed mixture mobile model modify mom moment monitor monkey monster month moon moral more morning mosquito mother motion motor mountain mouse move movie much muffin mule multiply muscle museum mushroom music must mutual myself mystery myth naive name napkin narrow nasty nation nature near neck need negative neglect neither nephew nerve nest net network neutral never news next nice night noble noise nominee noodle normal north nose notable note nothing notice novel now nuclear number nurse nut oak obey object oblige obscure observe obtain obvious occur ocean october odor off offer office often oil okay old olive olympic omit once one onion online only open opera opinion oppose option orange orbit orchard order ordinary organ orient original orphan ostrich other outdoor outer output outside oval oven over own owner oxygen oyster ozone pact paddle page pair palace palm panda panel panic panther paper parade parent park parrot party pass patch path patient patrol pattern pause pave payment peace peanut pear peasant pelican pen penalty pencil people pepper perfect permit person pet phone photo phrase physical piano picnic picture piece pig pigeon pill pilot pink pioneer pipe pistol pitch pizza place planet plastic plate play please pledge pluck plug plunge poem poet point polar pole police pond pony pool popular portion position possible post potato pottery poverty powder power practice praise predict prefer prepare present pretty prevent price pride primary print priority prison private prize problem process produce profit program project promote proof property prosper protect proud provide public pudding pull pulp pulse pumpkin punch pupil puppy purchase purity purpose purse push put puzzle pyramid quality quantum quarter question quick quit quiz quote rabbit raccoon race rack radar radio rail rain raise rally ramp ranch random range rapid rare rate rather raven raw razor ready real reason rebel rebuild recall receive recipe record recycle reduce reflect reform refuse region regret regular reject relax release relief rely remain remember remind remove render renew rent reopen repair repeat replace report require rescue resemble resist resource response result retire retreat return reunion reveal review reward rhythm rib ribbon rice rich ride ridge rifle right rigid ring riot ripple risk ritual rival river road roast robot robust rocket romance roof rookie room rose rotate rough round route royal rubber rude rug rule run runway rural sad saddle sadness safe sail salad salmon salon salt salute same sample sand satisfy satoshi sauce sausage save say scale scan scare scatter scene scheme school science scissors scorpion scout scrap screen script scrub sea search season seat second secret section security seed seek segment select sell seminar senior sense sentence series service session settle setup seven shadow shaft shallow share shed shell sheriff shield shift shine ship shiver shock shoe shoot shop short shoulder shove shrimp shrug shuffle shy sibling sick side siege sight sign silent silk silly silver similar simple since sing siren sister situate six size skate sketch ski skill skin skirt skull slab slam sleep slender slice slide slight slim slogan slot slow slush small smart smile smoke smooth snack snake snap sniff snow soap soccer social sock soda soft solar soldier solid solution solve someone song soon sorry sort soul sound soup source south space spare spatial spawn speak special speed spell spend sphere spice spider spike spin spirit split spoil sponsor spoon sport spot spray spread spring spy square squeeze squirrel stable stadium staff stage stairs stamp stand start state stay steak steel stem step stereo stick still sting stock stomach stone stool story stove strategy street strike strong struggle student stuff stumble style subject submit subway success such sudden suffer sugar suggest suit summer sun sunny sunset super supply supreme sure surface surge surprise surround survey suspect sustain swallow swamp swap swarm swear sweet swift swim swing switch sword symbol symptom syrup system table tackle tag tail talent talk tank tape target task taste tattoo taxi teach team tell ten tenant tennis tent term test text thank that theme then theory there they thing this thought three thrive throw thumb thunder ticket tide tiger tilt timber time tiny tip tired tissue title toast tobacco today toddler toe together toilet token tomato tomorrow tone tongue tonight tool tooth top topic topple torch tornado tortoise toss total tourist toward tower town toy track trade traffic tragic train transfer trap trash travel tray treat tree trend trial tribe trick trigger trim trip trophy trouble truck true truly trumpet trust truth try tube tuition tumble tuna tunnel turkey turn turtle twelve twenty twice twin twist two type typical ugly umbrella unable unaware uncle uncover under undo unfair unfold unhappy uniform unique unit universe unknown unlock until unusual unveil update upgrade uphold upon upper upset urban urge usage use used useful useless usual utility vacant vacuum vague valid valley valve van vanish vapor various vast vault vehicle velvet vendor venture venue verb verify version very vessel veteran viable vibrant vicious victory video view village vintage violin virtual virus visa visit visual vital vivid vocal voice void volcano volume vote voyage wage wagon wait walk wall walnut want warfare warm warrior wash wasp waste water wave way wealth weapon wear weasel weather web wedding weekend weird welcome west wet whale what wheat wheel when where whip whisper wide width wife wild will win window wine wing wink winner winter wire wisdom wise wish witness wolf woman wonder wood wool word work world worry worth wrap wreck wrestle wrist write wrong yard year yellow you young youth zebra zero zone zoo".split(" ");
  async function bip39ok(ws){ if(ws.length!==12) return false; var idx=ws.map(function(w){return WL.indexOf(w);}); if(idx.some(function(i){return i<0;})) return false; var bits=idx.map(function(i){return ("00000000000"+i.toString(2)).slice(-11);}).join(""); var ent=new Uint8Array(16); for(var i=0;i<16;i++) ent[i]=parseInt(bits.slice(i*8,i*8+8),2); var h=new Uint8Array(await crypto.subtle.digest("SHA-256",ent)); var cs=("00000000"+h[0].toString(2)).slice(-8).slice(0,4); return cs===bits.slice(128); }
  var hb = function (h) { var a = new Uint8Array(h.length / 2); for (var i = 0; i < a.length; i++) a[i] = parseInt(h.substr(i * 2, 2), 16); return a; };
  var bb = function (b) { var s = atob(b); var a = new Uint8Array(s.length); for (var i = 0; i < s.length; i++) a[i] = s.charCodeAt(i); return a; };

  function norm(lw) { return lw.join(" ").normalize("NFC").replace(/\s+/g, " ").trim(); }
  async function deriveDecrypt(phrase) {
    var km = await crypto.subtle.importKey("raw", enc.encode(phrase), "PBKDF2", false, ["deriveKey"]);
    var key = await crypto.subtle.deriveKey({ name: "PBKDF2", salt: hb(BLOB.salt), iterations: 250000, hash: "SHA-256" }, km, { name: "AES-GCM", length: 256 }, false, ["decrypt"]);
    try { return new TextDecoder().decode(await crypto.subtle.decrypt({ name: "AES-GCM", iv: hb(BLOB.iv) }, key, bb(BLOB.ct))); }
    catch (e) { return null; }
  }
  function reveal(text) {
    var doc = new DOMParser().parseFromString(text, "text/html");
    var box = document.getElementById("ln-reveal"); box.className = "reveal";
    box.replaceChildren.apply(box, Array.prototype.slice.call(doc.body.childNodes));
  }
  async function light() {
    darkEl.textContent = "";
    var words = inputs.map(function (x) { return x.value.trim().toLowerCase(); });
    var missing = [];
    for (var i = 0; i < 12; i++) { if (words[i].length === 0) missing.push(i); }
    if (missing.length === 0) {
      if (!(await bip39ok(words))) { darkEl.textContent = "a word is mistaken \u2014 these are not twelve that belong together."; return; }
      var pt = await deriveDecrypt(norm(words));
      if (pt === null) { darkEl.textContent = "the vessel stays dark."; return; }
      darkEl.textContent = "the lantern knows this hand."; reveal(pt); return;
    }
    if (missing.length > 1) {
      darkEl.textContent = "eleven of the twelve, and the lantern can search out the last for you \u2014 but " + missing.length + " are dark, and that it cannot. find more, then return.";
      return;
    }
    // exactly one missing: search the wordlist for the last mark (checksum-pruned, GCM-tag-confirmed; ~10s).
    var slot = missing[0], found = null;
    for (var wi = 0; wi < WL.length; wi++) {
      var cand = words.slice(); cand[slot] = WL[wi];
      if (!(await bip39ok(cand))) continue;
      var pt2 = await deriveDecrypt(norm(cand));
      if (pt2 !== null) { found = { word: WL[wi], pt: pt2 }; break; }
      if ((wi & 31) === 0) { darkEl.textContent = "searching the old wordlist for the missing mark \u2014 " + Math.round(wi * 100 / WL.length) + "%"; await new Promise(function (r) { setTimeout(r, 0); }); }
    }
    if (found) {
      inputs[slot].value = found.word; refresh();
      darkEl.textContent = "the missing mark was \u201c" + found.word + "\u201d \u2014 the lantern knows this hand.";
      reveal(found.pt);
    } else {
      darkEl.textContent = "every word was tried and none completed this hand \u2014 more than one mark is wrong, or the eleven are not in the order they were found.";
    }
  }
  document.getElementById("ln-light").addEventListener("click", light);
  refresh();
})();
</script>
