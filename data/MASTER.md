# SOON Sydney — Project Master File
*Read this first in every new session. One URL → full context.*

---

## 1. Live URLs
- **GitHub Pages (primary):** https://abutrym2609.github.io/soon-sydney/
- **Repo:** https://github.com/abutrym2609/soon-sydney
- **Netlify:** DISABLED (plan limit)

---

## 2. Project Overview

SOON (Sydney. Only One Neighbourhood.) is an AI-powered suburb intelligence platform for Sydney. Two interconnected products:

**A) Sydney Explorer** — interactive Leaflet map of 594 suburbs with large profile cards per suburb. Single-file HTML/JS app (`index.html`). Each suburb card is a standalone HTML file loaded in an iframe on click.

**B) Swipe Matcher** — Tinder-style suburb matching via watercolour illustration cards. User swipes 16 cards → cosine similarity match across all 594 suburbs → top 3 results with explanation. (`soon-swipe.html`)

---

## 3. File Structure (GitHub repo)

```
soon-sydney/
├── index.html              ← Main map (var CARDS={...} + GeoJSON)
├── soon-swipe.html         ← Swipe matcher
├── soon-entry-duo.html     ← Entry screen + audience selector
├── soon-menu-concept.html  ← Burger menu concept
├── soon-cabinet-concept.html ← Personal cabinet concept
├── soon-copy-system.html   ← Brand ToV guide
├── data/
│   ├── MASTER.md           ← THIS FILE
│   └── swipe-data.json     ← Tagged card vectors (update each session)
└── cards/
    ├── avalon-beach.html
    ├── glebe.html
    ├── surry-hills.html
    ├── riverwood.html
    ├── manly.html
    ├── bondi.html
    ├── mosman.html
    ├── kirribilli.html
    ├── paddington.html
    ├── maroubra.html
    ├── pyrmont.html
    ├── newtown.html
    ├── marrickville.html
    ├── leichhardt.html
    ├── palm-beach.html
    ├── mona-vale.html
    ├── warriewood.html
    ├── newport.html
    ├── point-piper.html
    ├── vaucluse.html
    ├── bayview.html
    ├── clareville.html
    ├── bilgola-plateau.html
    ├── scotland-island.html
    ├── lovett-bay.html
    └── church-point.html
```

---

## 4. Technical Stack

- **Map:** Leaflet.js (Canvas renderer, smoothFactor:0)
- **Fonts:** Cormorant Garamond + DM Sans (Google Fonts)
- **Weather:** Open-Meteo API (free, no key)
- **Marine:** Open-Meteo Marine API
- **Data:** ABS Census 2021, CoreLogic, BOCSAR, Domain
- **No build system** — pure vanilla HTML/CSS/JS, single files

### index.html Architecture
```javascript
var CARDS = { "SUBURB NAME": { house_p, unit_p, rent, growth3... } }
var GEO = { ...GeoJSON of 594 suburbs... }
var EMBEDDED = {}  // empty — all profiles are standalone files
```

### Profile Card Pattern
```javascript
// Back button (in every suburb card)
onclick="try{window.parent.closeIframeProfile();}
catch(e){window.parent.postMessage('closeIframeProfile','*');}"

// index.html listens:
window.addEventListener('message', function(e){
  if(e.data==='closeIframeProfile') closeIframeProfile();
});
```

### SAL Code Normalization
```python
str(int(float(x))).zfill(5)
```

### Map Pricing Rule
```python
median_p = max(house_p, unit_p)  # avoid apartment suburbs appearing cheap
```

---

## 5. Design System

### Palette
```css
--bg:    #F5F1E8   /* warm parchment */
--bg2:   #EDE8DC   /* slightly darker */
--dark:  #2D2A24   /* near-black */
--ink2:  rgba(45,42,36,.72)
--ink3:  #7A6D5E   /* muted brown */
--accent:#5C4A3A   /* dark warm brown */
--h:     rgba(45,42,36,.1)  /* hairline */
--gr:    #4a7a4a   /* green */
--am:    #8a6020   /* amber */
--rd:    #8a3030   /* red */
```

### Typography
- **Serif:** Cormorant Garamond (headings, hero, italic accents)
- **Sans:** DM Sans weight 200/300/400/500 only
- **Hero size:** clamp(36px, 5vw, 64px), font-style:italic, line-height:.88

### Card CSS Classes (from manly.html — the reference template)
`.canvas` → 12-column grid  
`.s-big` → span 3, large stat  
`.char-cell` → span 12, character quote  
`.lcat` → span 3, liveability category  
`.scores-cell` → span 12, 4 scores  
`.wh-col` → span 2, who lives here  
`.life-head` / `.le` → life section  
`.hob-head` / `.hob` → hobbies section  
`.v-head` / `.voice` → resident voices  
`.prop-dark` → dark property band  
`.comm-head` / `.dblock` → community  
`.upd-a` / `.upd-b` → 2026 update  

### Gallery Pattern (after hob section, before v-head)
```html
<!-- Gallery -->
<div style="grid-column:span 12;border-top:.5px solid var(--h);
display:grid;grid-template-columns:repeat(6,1fr);gap:0">
  <div style="border-right:.5px solid var(--h);display:flex;flex-direction:column">
    <img src="data:image/png;base64,..." 
         style="width:100%;height:auto;display:block;filter:saturate(.85)">
    <div style="padding:10px 14px 12px;background:var(--bg);font-size:7px;
    letter-spacing:.18em;text-transform:uppercase;color:var(--ink3);flex:1">
      Caption text
    </div>
  </div>
  ...6 total...
</div>
```

**Gallery captions rule:** Lifestyle only, no suburb name.
- ✓ "Sunrise yoga"
- ✓ "Jazz in a small bar"  
- ✗ "Sunrise yoga — Anzac Bridge view"

**Gallery order:** Must match hob sections left-to-right.

---

## 6. Brand Voice (ToV)

### Core Principle
Sound like a knowledgeable friend, not an agent or algorithm.

### Approved Entry Copy (entry screen)
> *Sydney doesn't reveal itself all at once.*  
> *It gives you one neighbourhood first — the one you could afford,*  
> *the one a friend mentioned, the one that was available*  
> *when you needed somewhere to land.*  
> *Then, slowly, it shows you the rest.*  
> *We just make that part happen a little sooner. Hence the name.*

### Three Audience Cards (approved)
1. *"I'm still on that first neighbourhood. The one I landed in."*
2. *"I've been here long enough to suspect there's a version of Sydney I haven't seen yet."*
3. *"I want to know which neighbourhoods Sydney is about to reveal to everyone else."*

### Card Structure (10 sections, no hob)
01. Identity Hero + Character
02. Standout Facts (3)
03. District Portrait (editorial, two-column)
04. Domain Liveability (15 parameters)
05. Who Lives Here (4 archetypes)
06. Life in the Suburb (6 items)
07. **Gallery (6 illustrations)** ← replaces hob section
08. Resident Voices (3)
09. Real Estate
10. Community Data + 2026 Update

### Suburb Profile Rules
- Name the paradox (every suburb has one)
- Write about Tuesdays, not Saturdays
- Never use: vibrant, bustling, charming, eclectic, affordable, convenient, stunning, trendy, up-and-coming, hidden gem
- Honest about the bad (airport noise, traffic, crime)
- Resident voices: specific, self-aware, not generic

### Portrait Format
Two-column, full-width, 40px padding each side.  
Left: observational editorial prose.  
Right: continuation + paradox statement.  
Font: Cormorant Garamond italic 15px, line-height 1.8.

---

## 7. Swipe Matcher — Tag System

### 8 Axes (each card scored 0–1)

```json
{
  "environment": "coastal | urban | nature | suburban",
  "time":        "morning | daytime | evening | night",
  "pace":        "slow | active | social | quiet",
  "activity":    "wellness | sport | culture | food | work | family | leisure",
  "social":      "solo | couple | group | community",
  "income":      "premium | mid | grounded",
  "vibe":        "creative | bohemian | professional | outdoorsy | homely",
  "transport":   "walkable | car | water | transit"
}
```

### Scoring Rules
- Swipe right → +2 to matched axes
- Swipe left → -1 to matched axes
- After 16 cards → cosine_similarity(user_vector, suburb_vector)
- Suburb vector = aggregate of 6 card vectors + ABS income/age data

### Next Card Selection
```
next = max(info_gain * 0.6 + suburb_balance * 0.4)
info_gain = how much this card resolves uncertain axes
suburb_balance = 1 - (how often this suburb has appeared)
```

---

## 8. Cards Built — Property Data

| Suburb | House | Unit | Rent | Growth | Percentile |
|--------|-------|------|------|--------|------------|
| Avalon Beach | $3.15M | $1.28M | $850/wk | +22% | Top 5% |
| Bayview | $3.25M | $1.45M | — | +38% | Top 5% |
| Bilgola Plateau | $2.85M | $1.25M | — | +22% | Top 5% |
| Bondi Beach | $4.2M | $1.65M | — | +35% | Top 2% |
| Church Point | $3.15M | $1.45M | — | +22% | Top 5% |
| Clareville | $4.15M | $1.65M | — | +28% | Top 2% |
| Glebe | $2.85M | $1.32M | $635/wk | +39% | Top 15% |
| Kirribilli | $4.25M | $1.65M | — | +12% | Top 2% |
| Leichhardt | $2.25M | $1.15M | — | +35% | Top 15% |
| Lovett Bay | $2.85M | — | $1,100/wk | +22% | Top 5% |
| Manly | $4.85M | $2.15M | — | +32% | Top 2% |
| Maroubra | $2.85M | $1.15M | — | +32% | Top 10% |
| Marrickville | $2.25M | $980k | — | +38% | Top 10% |
| Mona Vale | $2.85M | $1.45M | $785/wk | +22% | Top 10% |
| Mosman | $3.95M | $1.42M | — | +18% | Top 2% |
| Newtown | $2.15M | $985k | — | +22% | Top 5% |
| Newport | $2.95M | $1.35M | — | +32% | Top 5% |
| Paddington | $4.1M | $1.45M | — | +22% | Top 2% |
| Palm Beach | $5.85M | $2.1M | — | +22% | 99th pct |
| Point Piper | $24.5M | $3.45M | — | +65% | Top 0.1% |
| Pyrmont | $2.45M | $1.38M | — | +18% | Top 5% |
| Riverwood | $1.38M | $745k | $550/wk | +32% | 45th pct |
| Scotland Island | $1.95M | — | — | +22% | Top 15% |
| Surry Hills | $2.66M | $1.48M | $850/wk | +22% | Top 5% |
| Vaucluse | $6.2M | $1.65M | — | +22% | Top 1% |
| Warriewood | $2.45M | $1.35M | $780/wk | +32% | Top 15% |

---

## 9. Session Protocol

### Start of every session
1. `web_fetch` this MASTER.md
2. `web_fetch` data/swipe-data.json
3. Ask user: which suburb today?

### Build a new suburb card
1. Read brief + illustrations
2. Build HTML card (use manly.html as template)
   - NO hob section (How They Spend Time) — replaced by 6-image gallery
   - Gallery goes between life section and resident voices
   - Captions: lifestyle only, no suburb name (e.g. "Sunrise yoga" not "Sunrise yoga — Anzac Bridge")
3. Tag 6 gallery illustrations → add to swipe-data.json
4. Add suburb vector to swipe-data.json
5. Update CARDS entry in cards.js
6. Output: suburb.html + cards.js + swipe-data.json

### JS Safety Rules
- Never use template literals in Python heredocs → use string concatenation
- Always run `node --check /tmp/file.js` before delivering
- Gallery removal: never use regex with `.*?</div>` on large files → use position-based slicing
- Gallery insertion: always insert via `html[:idx] + new_content + html[idx:]`

### Known Pitfalls
- Double-escaped regex in Python→JS: use `[(]` instead of `\(`
- Optional chaining `?.` breaks some environments: use explicit null check
- Gallery regex removal corrupts file: use `re.sub(..., count=1)` carefully, verify size after

---

## 10. SOON Masterspreadsheet (separate track)

- 698 suburbs, 300+ attributes, ABS Census 2021 verification
- Working file: `/home/claude/SOON_Masterspreadsheet_FILLED.csv`
- Known systematic errors: Religion_Christian_pct near-100%, ancestry rankings wrong, Born_Overseas_pct miscalculated
- Excluded fields: Separated_pct, Religion_EasternOrthodox_pct, Cancer_pct, Dementia_pct
- Ambiguous fields (left unchanged): Works_Fulltime_pct, Works_LongHours_pct, Mortgage_Stress_pct, Median_Personal_Income_Weekly_Census
