# CLAUDE.md

Behavioral guidelines to reduce common LLM coding mistakes. Merge with project-specific instructions as needed.

**Tradeoff:** These guidelines bias toward caution over speed. For trivial tasks, use judgment.

## 1. Think Before Coding

**Don't assume. Don't hide confusion. Surface tradeoffs.**

Before implementing:
- State your assumptions explicitly. If uncertain, ask.
- If multiple interpretations exist, present them - don't pick silently.
- If a simpler approach exists, say so. Push back when warranted.
- If something is unclear, stop. Name what's confusing. Ask.

## 2. Simplicity First

**Minimum code that solves the problem. Nothing speculative.**

- No features beyond what was asked.
- No abstractions for single-use code.
- No "flexibility" or "configurability" that wasn't requested.
- No error handling for impossible scenarios.
- If you write 200 lines and it could be 50, rewrite it.

Ask yourself: "Would a senior engineer say this is overcomplicated?" If yes, simplify.

## 3. Surgical Changes

**Touch only what you must. Clean up only your own mess.**

When editing existing code:
- Don't "improve" adjacent code, comments, or formatting.
- Don't refactor things that aren't broken.
- Match existing style, even if you'd do it differently.
- If you notice unrelated dead code, mention it - don't delete it.

When your changes create orphans:
- Remove imports/variables/functions that YOUR changes made unused.
- Don't remove pre-existing dead code unless asked.

The test: Every changed line should trace directly to the user's request.

## 4. Goal-Driven Execution

**Define success criteria. Loop until verified.**

Transform tasks into verifiable goals:
- "Add validation" → "Write tests for invalid inputs, then make them pass"
- "Fix the bug" → "Write a test that reproduces it, then make it pass"
- "Refactor X" → "Ensure tests pass before and after"

For multi-step tasks, state a brief plan:
```
1. [Step] → verify: [check]
2. [Step] → verify: [check]
3. [Step] → verify: [check]
```

Strong success criteria let you loop independently. Weak criteria ("make it work") require constant clarification.

---

**These guidelines are working if:** fewer unnecessary changes in diffs, fewer rewrites due to overcomplication, and clarifying questions come before implementation rather than after mistakes.

---

# Projet : GO RENOV — Stratégie MaPrimeRénov' 2025-2026

## Contexte métier

Page web de présentation de la **stratégie commerciale de GO RENOV**, mandataire ANAH et RGE
sur le marché de la rénovation énergétique. Le document analyse les contraintes du dispositif
**MaPrimeRénov'** (suspension du parcours global en 2025, baisse budgétaire 2026) et positionne
GO RENOV sur les **mono-gestes** (PAC, isolation, audit, VMC), les **leads via simulateur** et
les **abonnements partenaires**. C'est un support stratégique/commercial — les chiffres
(budgets, aides, commissions, projections de CA) sont des données métier, pas des valeurs
techniques arbitraires. **Ne pas modifier un montant ou une date sans demander.**

## Stack & structure

- **Un seul fichier : `index.html`** — HTML + CSS + JS, tout est inline. Pas de build, pas de
  bundler, pas de framework. On ouvre le fichier directement dans le navigateur.
- **Dépendances via CDN** (pas de `node_modules`) :
  - Tailwind CSS 2.2.19 (utilitaires de classe)
  - Font Awesome 6.4.0 (icônes `<i class="fas fa-...">`)
  - Chart.js (graphiques `<canvas>`)
  - Police Inter (Google Fonts)
- **JavaScript vanilla** dans la balise `<script>` en bas de page : simulateur de revenus
  (sliders → `updateCalculations()`) et 3 graphiques Chart.js initialisés au `load`.
- **Langue : français.** Tout le contenu visible, les commentaires et les libellés sont en
  français. Garder cette langue.

## Conventions

- **Style :** utiliser les classes Tailwind existantes et les classes CSS personnalisées déjà
  définies (`.card-hover`, `.gradient-bg`, `.highlight-box`, `.success-box`, `.warning-box`,
  `.info-box`, `.slider`, `.results-card`). Réutiliser avant d'inventer.
- **Cohérence du simulateur :** chaque slider a un `id`, un `<span>` d'affichage de valeur et
  une variable JS globale. Si on ajoute un paramètre, respecter ce trio et le brancher dans
  `updateCalculations()`. Les revenus globaux mensuels sont calculés comme `globalCount * 125`
  (≈ 1 500 €/dossier ÷ 12).
- **Graphiques :** les données chiffrées sont codées en dur dans `new Chart(...)`. Si une
  donnée métier change ailleurs dans la page, vérifier qu'elle reste cohérente dans le
  graphique correspondant.
- **Responsive :** mise en page mobile gérée par les breakpoints Tailwind (`lg:`, `xl:`,
  `md:`) et la media query `@media (max-width: 768px)`. Tester l'affichage mobile après un
  changement de layout.

## Vérification

Pas de tests automatisés. Pour valider un changement : ouvrir `index.html` dans un navigateur
et contrôler visuellement que les sliders mettent à jour les chiffres, que les 3 graphiques
s'affichent, et que la page reste responsive.
