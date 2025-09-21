
---

## `docs/Playbook-CI-Tests.md` (pédago + pense-bête)
```md
# Playbook CI + Tests + Lint

Ce document t’explique **à quoi sert chaque étape**, **où elle vit** et **comment s’en servir**.

---

## 1) Les rôles

- **package.json** : liste des **scripts** que tu lances localement et que la CI relance automatiquement.
- **CI (GitHub Actions)** : exécute tes scripts sur un serveur GitHub à chaque `push/PR`.
- **Lint (ESLint)** : vérifie la qualité du code **sans l’exécuter** (style + pièges fréquents).
- **Prettier** : formatage **uniforme** du code (espaces, guillemets, etc.).
- **Tests (Vitest)** : exécutent ton code pour prouver son comportement.
- **Build** : fabrique le livrable (optionnel pour les libs/scripts).

---

## 2) Comment ça s’enchaîne

1. **Dev** : tu écris/édites du code.
2. **Local** : `npm run lint` → `npm run test`.
3. **Push/PR** : la CI relance **les mêmes commandes** sur un serveur propre.
4. Si tout est **vert** → merge en confiance. Si **rouge** → corrige avant de merger.

---

## 3) Lint vs Format

- **Lint** (ESLint) = règles de qualité et d’anti-erreurs (ex.: variable non utilisée).
- **Format** (Prettier) = style visuel (guillemets, trailing commas, largeur de ligne).

> Tips : pour corriger automatiquement le style, tu peux ajouter `npm run format:fix` = `prettier -w .`.

---

## 4) Tests (Vitest)

- Fichiers de test : `tests/*.test.js`.
- Commande : `npm run test` (en CI) ou `npx vitest` (mode dev interactif).

Exemple :
```js
import { describe, it, expect } from 'vitest';
import { sum } from '../src/sum.js';

describe('sum', () => {
  it('adds two numbers', () => {
    expect(sum(2, 3)).toBe(5);
  });
});
