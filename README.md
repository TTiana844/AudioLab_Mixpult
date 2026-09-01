# Mixpult úlohy — odovzdávací systém s AI hodnotením

Statická webová aplikácia (`index.html`) + jedna Vercel serverless funkcia
(`api/evaluate.js`), ktorá bezpečne volá Anthropic API a vracia AI hodnotenie
odovzdanej práce. API kľúč sa nikdy neposiela do prehliadača — žije len na
serveri ako premenná prostredia.

## Štruktúra projektu

```
.
├── index.html          # celá aplikácia (frontend, 10 cvičení, tutoriály)
├── api/
│   └── evaluate.js      # serverless funkcia - volá Anthropic API
├── package.json
├── .gitignore
└── README.md
```

## 1. Nahratie na GitHub

Ak ešte nemáš repozitár:

```bash
git init
git add .
git commit -m "Mixpult úlohy s AI hodnotením"
git branch -M main
git remote add origin https://github.com/<tvoj-ucet>/<repo>.git
git push -u origin main
```

Ak repozitár už existuje (napr. ten istý, čo používa `audioLabGithubVercel.html`),
skopíruj do neho `index.html`, priečinok `api/` a `package.json` a commitni/pushni.

## 2. Import projektu do Vercelu

1. Choď na [vercel.com](https://vercel.com) → **Add New → Project**.
2. Vyber svoj GitHub repozitár.
3. Framework Preset nechaj na **Other** (nie je to Next.js/React projekt).
4. Build Command a Output Directory nechaj prázdne/predvolené — je to statický
   `index.html` + `api/` priečinok, Vercel ho rozpozná automaticky.

## 3. Nastavenie API kľúča (najdôležitejší krok)

1. V projekte na Vercel choď do **Settings → Environment Variables**.
2. Pridaj premennú:
   - **Name:** `ANTHROPIC_API_KEY`
   - **Value:** tvoj skutočný kľúč z [console.anthropic.com](https://console.anthropic.com/settings/keys)
   - **Environment:** zaškrtni Production, Preview aj Development.
3. Ulož a spusti **Redeploy** (Deployments → tri bodky pri poslednom deploji →
   Redeploy) — premenná sa načíta až pri novom builde.

Kľúč sa použije len vnútri `api/evaluate.js` (`process.env.ANTHROPIC_API_KEY`)
na serveri, nikdy nie je súčasťou `index.html` ani JS kódu, ktorý beží
v prehliadači.

## 4. Otestovanie

Po deployi otvor pridelenú `*.vercel.app` adresu, vyber ľubovoľné cvičenie,
zaškrtni pár bodov kontrolného zoznamu, napíš meno a komentár a klikni na
**„Odoslať na AI hodnotenie“**. Aplikácia zavolá `/api/evaluate`, ktorý
zavolá Anthropic API a vráti skutočné AI hodnotenie (známka, percentá,
kritériá, silné stránky, odporúčania).

Ak API z akéhokoľvek dôvodu zlyhá (chýbajúci kľúč, výpadok, chyba siete),
aplikácia sa automaticky prepne na lokálny náhradný odhad podľa splnenia
kontrolného zoznamu (funkcia `evaluateWork()` v `index.html`), takže appka
nikdy nezamrzne ani nespadne.

## Aktualizácia obsahu

Keďže je to jeden statický `index.html`, akákoľvek ďalšia úprava (texty,
cvičenia, tutoriály) = uprav súbor lokálne → commit → push. Vercel spraví
nový deploy automaticky pri každom pushi do sledovanej vetvy.

## Poznámka k modelu

Serverless funkcia aj frontend používajú `claude-sonnet-4-20250514` ako
predvolený model (rovnako ako referenčný projekt). Ak budeš neskôr chcieť
prejsť na iný model, stačí zmeniť reťazec `model` v `index.html`
(funkcia `submitWork`) — `api/evaluate.js` model len prepošle ďalej, nemá ho
natvrdo zakódovaný.
   
