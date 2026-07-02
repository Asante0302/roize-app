# DESIGN.md — Roize (v3 · ancré Blow Up + deep research)

> Direction figée : **Blow Up** (forme/onboarding) + **Opal** (gamification/paywall). Web premium, **iOS-only**. Anti-slop.
> v3 = v2 + deep research (color budget, named elevation, glass restreint, grain, line icons, motion). Source produit : `roize/SPEC.md`. Flow : `roize/design/ONBOARDING.md`.

---

## 1. Overview
Roize = copilote business quotidien du solopreneur. Premium, électrique, en contrôle.

**Le principe central (la règle d'or visuelle)** : **fond quasi noir + UNE source de lumière = le halo aurora en HAUT de l'écran.** La couleur est une invitée, pas le papier peint.
- **Budget couleur par écran : ~85% noir · ~12% halo aurora · ~3% lime.** Si deux zones sont saturées en même temps → ça fait cheap.
- **Signature Roize = le lime** (action / croissance / €). C'est ce qu'on touche. Le halo, c'est l'ambiance.

## 2. Colors
```
# base + RAMPE D'ÉLÉVATION nommée (pas d'opacités au hasard = tell n°1 de l'IA)
bg:          #0A0A0F      surface-1: #121218      surface-2: #16161E      surface-3: #1C1C26
ink:         #050409      hairline:  rgba(255,255,255,.07)     hairline-2: rgba(255,255,255,.12)

# texte (contraste AA obligatoire)
text:        #F6F4FF      text-2: rgba(255,255,255,.72)      text-3: rgba(255,255,255,.50)   (jamais < .40)

# HALO "Aurora" — UNE seule fois, en haut, jamais répété mid-screen
aurora:      linear-gradient(100deg,#4F6BFF,#8B3CFF,#E24BD0)        (bleu→violet→magenta)
halo:        radial-gradient(120% 70% at 50% -8%, rgba(139,60,255,.5), rgba(79,107,255,.16) 40%, transparent 70%)  + blur 80-120px

# SIGNATURE (rare)
lime:   #B6FF3C   lime-ink:#122400      emerald:#2BD67B (€/revenu)
ok:     #6EE7A8   ok-bg:rgba(43,214,123,.08)   ok-line:rgba(43,214,123,.25)
```
**Règles** : lime = CTA principal, le score/chiffre de croissance, streak, deltas positifs. **Jamais** lime en fond, ni en texte courant, ni sur 2 éléments concurrents. Halo = 1 source en haut, fond vers noir pur à ~40% de hauteur, **jamais** répété. Aurora aussi sur : bordure d'élément sélectionné, progress, courbes, anneau de score.

## 3. Typography
- **Clash Display** (titres, 600/700, blanc) · **Satoshi** (corps/UI) · **Geist Mono** (TOUS les chiffres).
- Chiffres : `font-variant-numeric: tabular-nums` (+ `"tnum","zero"`). Gros titres Clash : `letter-spacing:-.02em`. Petits labels mono caps : tracking léger positif.
```
display: Clash 700 · 34px/-1px      h1: Clash 600 · 27px      h2: Clash 600 · 21px
body: Satoshi 500 · 16px            body-2: Satoshi 500 · 14px (text-2)
micro: Satoshi 700 · 11.5px · +1.4px · UPPERCASE (labels)
metric: Geist Mono 600 · 40px (tabular)     metric-sm: Geist Mono 600 · 15px
```
CDN : Fontshare (Clash, Satoshi) + Google Fonts (Geist Mono).

## 4. Layout
iPhone, safe-area haute. Barre de progression fine + chevron `<` en haut. **Halo aurora** derrière le haut. Contenu aligné gauche. Marges ext ≥ 22px, respiration verticale généreuse. Échelle d'espace : 4/8/12/16/24/32/48.

## 5. Elevation & Depth (verre 2026 restreint, pas 2021 heavy-blur)
```
.card{ background:rgba(255,255,255,.04); backdrop-filter:blur(20px) saturate(120%);
  border:1px solid var(--hairline);  /* hairline COMPLET, jamais une barre d'accent colorée */
  border-radius:20px;
  box-shadow: inset 0 1px 0 rgba(255,255,255,.06),  /* "lèvre" de verre en haut */
              0 20px 40px -22px rgba(0,0,0,.7); }     /* UNE ombre douce, vers le bas, neutre */
```
- **Une seule ombre** par défaut, neutre (jamais colorée partout = tell IA).
- **Glow lime** = réservé à UN élément hero/écran (l'anneau de score, le CTA actif) : `0 0 40px -8px rgba(182,255,60,.4)`. Le glow est un événement, pas une déco.
- **Grain de film** (anti-banding OLED + côté pellicule) : overlay SVG `feTurbulence`, opacity .03–.06, `mix-blend-mode:overlay`.
- Sélection = bordure `aurora` (1.5px, technique padding-box/border-box).

## 6. Shapes
`pill:999 · card:20-24 · input:16 · chip:12`. Squircles. **Max 2 niveaux d'imbrication.** Pas de barre d'accent 4px.

## 7. Components
```
btn-primary (action):  pilule SOLIDE lime · texte lime-ink · le SEUL bouton plein de l'écran · glow lime léger
btn-aurora (continuer/marque):  pilule · fond bg · bordure dégradée aurora (mask padding/border-box) · texte blanc
btn-white (welcome):   pilule blanche pleine · texte noir (cf. "Suivant" Blow Up)
btn-ghost:  transparent · text-3

option-card:  surface-1 · hairline complet · radius 18 · ICÔNE LIGNE dans un carré (Lucide/Phosphor, PAS d'emoji)
  selected:   bordure aurora + check lime + carré-icône surligné · feedback : spring + (haptique simulée scale .97)
feedback-ok:  encadré translucide ok-bg/ok-line · texte ok · apparait sous le bon choix

progress:  fill lime sur piste rgba(255,255,255,.06) · glow lime au bord avant · s'anime au montage (0→%)
report:    anneau de score conic (aurora→lime) + trou radial · gros chiffre Geist Mono lime au centre, COUNT-UP
           + courbe de croissance : 1 ligne lime, fill lime→transparent dessous, pointillé "projeté", stroke-draw animé
           + jalons = petites gemmes sur la courbe
stat-chip:  Geist Mono (tabular) + label micro caps
list-row (€): pastille + titre + fourchette € (Geist Mono, à droite) + chevron déplier (cf. monétisation Blow Up)
paywall:   avant/après (2 mini-courbes) · 3 plans, ANNUEL pré-coché + badge "-X%" lime · timeline d'essai claire
           · PRIX TOTAL réel affiché (ne pas cacher derrière le €/jour) · bandeau note ★ + nb d'avis + logos paiement
tab-bar:   barre pilule flottante (marge bas), verre + hairline · actif = icône LIGNE lime + point/glow · inactif text-3
cele:      milestone → gemme 3D qui spring-in + burst de particules lime + count-up du score + haptique succès
sheet:     bottom-sheet surface-3, radius haut 26, arrive en spring
empty:     gemme 3D atténuée + 1 phrase + 1 CTA lime (jamais "Aucune donnée" gris)
```

## 8. Motion
Springs (jamais linéaire) : `cubic-bezier(.22,1,.36,1)`, léger overshoot. **Reveals décalés** (items fade+rise 8-16px, stagger 40-60ms). **Count-up** sur chaque chiffre. **Jauges/barres** s'animent de 0. **Halo respire** très lentement (8-12s). Tap → scale .97 + spring (tactilité). Respecter `prefers-reduced-motion`.

## 9. Do / Don't (anti-slop)
**DON'T** : ❌ Inter/Roboto · ❌ Space Grotesk+serif italique · ❌ violet/dégradé partout · ❌ glow coloré + box-shadow colorée sur tout · ❌ glass lourd partout · ❌ barre d'accent colorée sur cartes · ❌ **emoji en icônes d'UI/nav** (→ set d'icônes ligne) · ❌ cartes identiques icon-on-top répétées · ❌ rangées 1-2-3 / bandeaux de stats génériques · ❌ texte gris < AA · ❌ tout en MAJUSCULES · ❌ ombres/blur qui dérivent par composant · ❌ tout qui pop en même temps · ❌ cacher le prix réel derrière le €/jour.
**DO** : noir + 1 halo en haut · lime rare = action/€ · Geist Mono tabular pour les chiffres · rampe d'élévation nommée · icônes ligne · grain · springs + count-ups · 1 opinion forte par dimension, tenue.

**Méta-règle** : écrire le spec AVANT de builder. Toute zone laissée vague, le modèle la remplit avec le défaut « purple glow ».

## 10. Ressources
Fonts : fontshare.com (Clash, Satoshi) · fonts.google.com/specimen/Geist+Mono. Composants : 21st.dev. 3D gemmes : sketchfab.com (.glb) via `<model-viewer>` (no-build) ou spline.design. Grain : css-tricks.com/grainy-gradients. Icônes : Lucide / Phosphor.

---
*Réfs analysées : `roize/refs/blow-up/` (welcome 3D, social proof, quiz icônes-ligne, loading+question, profil, report, monétisation, email, paywall timer). Recherche : voir mémoire [[reference-claude-design]].*
