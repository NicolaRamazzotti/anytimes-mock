# Anytimes — Mock Sito Istituzionale

Progetto mock del sito marketing per **Anytimes**, gestionale SaaS per centri sportivi (padel, tennis, fitness).

---

## Struttura file

```
index.html          Homepage con hero, video mock, features bento, AI section, pricing, CTA
funzionalita.html   Pagina funzionalità con bento grid per categoria + sezione AI
club.html           Elenco club con filtro/ricerca, card prenota + badge App Store/Play
prezzi.html         Piani con toggle mensile/annuale, tabella comparativa, FAQ
chi-siamo.html      Pagina about team
blog.html           Blog con featured post, griglia articoli, newsletter
contatti.html       Form contatto + info
shared.css          Design system condiviso (variabili, nav, footer, bottoni, utility)
assets/logo.png     Logo icona brand (svg-like, sfondo trasparente)
assets/video.mp4    Video demo dashboard — usato nell'hero della homepage
```

---

## Design system

### Font
- **Display / Titoli**: `Syne` (wght 400–800) — `var(--font-display)`
- **Body / UI**: `Inter` (wght 300–500) — `var(--font-body)`

### Palette
| Token | Valore | Uso |
|---|---|---|
| `--brand` | `#F7583F` | CTA primari, accenti |
| `--brand-dark` | `#D43E27` | Hover bottoni brand |
| `--brand-mid` | `#F87B66` | Testi su dark |
| `--dark` | `#0D0D0D` | Background principale |
| `--dark-card` | `#161616` | Card su sfondo dark |
| `--dark-2` | `#1A1A1A` | Strip secondarie |
| `--off-white` | `#F5F4F0` | Sezioni chiare |
| `--border-dark` | `rgba(255,255,255,0.09)` | Bordi su dark |
| `--border-light` | `#E8E6E0` | Bordi su light |

### Border radius
- `--r`: 12px (bottoni, input)
- `--r-lg`: 20px (card medie)
- `--r-xl`: 28px (card grandi, sezioni)

---

## Convenzioni HTML/CSS

- **Nessun framework CSS** — tutto vanilla CSS in `shared.css` + `<style>` page-level
- **Nomi classi**: BEM-like abbreviato (es. `.cc-body`, `.fp-title`, `.sf-btn`)
- **Animazioni entrata**: classe `.fade-up` o `.feat-card` con `opacity:0 + translateY(14px)` → aggiunta `.visible` via `IntersectionObserver`
- **Sezioni alternate**: `background:var(--dark)` e `background:var(--off-white)` a strati
- **Responsive**: breakpoint a `960px` (tablet) e `600px` (mobile), media query in fondo a ogni `<style>`
- **JS**: vanilla, inline in fondo al body. Solo per animazioni, filtri e toggle — niente dipendenze esterne

---

## Componenti riutilizzabili (in shared.css)

| Classe | Descrizione |
|---|---|
| `.btn-primary` | Bottone brand arancio |
| `.btn-secondary` | Bottone ghost su dark |
| `.btn-ghost-outline` | Outline brand |
| `.btn-nav-cta` | Bottone nav "Richiedi demo" |
| `.page-hero` | Hero centrato per pagine interne |
| `.cta-section` + `.cta-inner` | Sezione CTA finale dark con glow |
| `.section` + `.inner` | Container sezione (max 1200px) |
| `.eyebrow` | Label uppercase brand sopra titolo |
| `.s-title` | Titolo sezione (Syne 800) |
| `.s-sub` | Sottotitolo sezione |
| `.logo-strip` | Strip loghi clienti |
| `footer` + `.footer-inner` | Footer standard |

---

## Nav — voci e ordine

```
Logo | Funzionalità | Club | Chi siamo | Prezzi | Blog   [Accedi]  [Richiedi demo →]
```

- Link attivo: classe `active` sul `<a>` corrispondente
- Logo: `<a class="logo"><img src="assets/logo.png">any<span>times</span></a>`

---

## Pagina Club (`club.html`)

- Ogni `<div class="club-card">` ha attributi `data-sports="padel tennis..."` e `data-name="nome città..."` per il filtro JS
- Il bottone **Prenota** linka alla webapp del circolo (`href` da aggiornare con URL reale)
- I badge **iOS / Android** nella cover e il bottone **App** nelle azioni vanno aggiunti solo ai club che hanno l'app mobile — basta aggiungere il blocco `.cc-app-badges` e `.cc-btn-app` con gli URL reali di App Store / Google Play

---

## Pagina Funzionalità — moduli AI

I 4 moduli AI sono nella sezione `#ai` di `funzionalita.html`:
1. **AI Insight** — analisi dati struttura, insight settimanali
2. **Player Metrics** — statistiche avanzate per giocatore
3. **Smart Match** — abbinamento match per skill level
4. **Company Insight** — KPI, forecast, churn, LTV

---

## Hero homepage — video

La finestra stile macOS nell'hero usa:
```html
<div class="dash-shell">
  <div class="dash-titlebar">…</div>
  <video autoplay muted loop playsinline>
    <source src="assets/video.mp4" type="video/mp4">
  </video>
</div>
```
- `muted` è obbligatorio per l'autoplay su tutti i browser
- Il file sorgente è `assets/video.mp4`

---

## Sistema tema chiaro/scuro

- Toggle `data-theme="dark|light"` su `<html>`, persistito in `localStorage('at-theme')`, inizializzato in `<head>` prima del CSS
- **`shared.css`** contiene i token di override in `[data-theme="light"] :root { … }` + override component-level
- **Regola fondamentale**: ogni volta che si crea una nuova sezione bisogna considerare **entrambi i temi**
  - Se la sezione usa `background:var(--dark)` o `background:var(--off-white)` → si adatta automaticamente ai token
  - Se la sezione usa un **background hardcoded** (es. `background:#0D0D0D`, `#06040e`, gradiente custom) → è una **dark island fissa** → aggiungere sempre nel blocco `[data-theme="light"]` della pagina gli override espliciti con `!important` per testi/bordi
  - Se la sezione usa `background:var(--off-white)` e ha testi con colori espliciti inline (es. `color:rgba(13,13,13,0.6)`) → sostituire con variabili CSS (`var(--text-muted-dark)`, `var(--dark)`) che si aggiornano col tema
- **Checklist per ogni nuova sezione**:
  1. Il background usa token CSS o è hardcoded?
  2. I testi usano `color:var(--white)` / `var(--dark)` o colori letterali?
  3. Bordi e separatori usano `var(--border-dark)` / `var(--border-light)`?
  4. Se hardcoded: aggiungere override nel blocco `[data-theme="light"]` page-level

---

## Mock UI (screen recording)

Cartella `mock/` — pagine standalone per la registrazione video dei singoli moduli dell'app. Ogni mock:
- È **completamente self-contained** (CSS + JS inline, zero dipendenze locali tranne le font Google)
- Path asset: percorso relativo da `mock/<nome>/` → `../../assets/`
- Ha `<meta name="viewport" content="width=1440">` — aprire il browser a **1440 × 900** per recording MacBook ideale
- Sfondo: `#08080C` con glow radiale brand, griglia di punti sottile
- Le animazioni girano in loop automatico, non serve input utente

```
mock/
  agenda/
    index.html    Dashboard + agenda multi-campo (animazione blocchi prenotazione + contatori)
```

### Come aggiungere un nuovo mock
1. Crea `mock/<nome>/index.html`
2. Estrai HTML + CSS + JS dal componente corrispondente nel sito
3. Adatta i path asset (`../../assets/...`)
4. Mantieni `<meta name="viewport" content="width=1440">`
5. Scrivi qui sotto il mock nella lista

### Mock pianificati
- [ ] `mock/app-mobile/` — schermata app mobile (iOS style)
- [ ] `mock/tornei/` — vista gestione torneo a tabellone
- [ ] `mock/stats/` — dashboard analytics / report

---

## Todo / Next steps

- [ ] Sostituire i `href="#"` delle card club con URL reali (webapp + App Store + Play Store)
- [ ] Aggiungere immagini/foto reali dei club in `assets/clubs/`
- [ ] Collegare form contatti a backend / Formspree
- [ ] SEO: meta description e og:image per ogni pagina
- [ ] Favicon dal logo
