# Playbook CI + Tests + Lint

Ce pense-bête explique **qui fait quoi**, **dans quel ordre** et **comment réagir** pour garder un projet Node/JS en bonne santé.

---

## 1. Les rôles
- **`package.json`** : catalogue des scripts qui servent à piloter lint, tests, build… localement et en CI.
- **CI (GitHub Actions)** : relance ces scripts à chaque `push`/`pull_request` pour vérifier qu’un commit neuf passe dans un environnement propre.
- **ESLint (lint)** : analyse statiquement ton code pour détecter bugs/anti-patterns sans l’exécuter.
- **Prettier (format)** : remet en forme le code pour éviter les guerres de style.
- **Vitest (tests)** : exécute les specs pour prouver que les fonctionnalités marchent.
- **Build** : prépare le livrable final (bundle, transpilation, etc.). Optionnel mais pratique pour détecter les surprises de prod.

---

## 2. L’enchaînement (local → CI)
1. **Tu développes** une fonctionnalité ou fixes un bug.
2. **Local** : lance `npm run lint`, puis `npm run test` (et `npm run typecheck`/`npm run build` si dispo).
3. **Commit & push** : la CI GitHub redémarre sur une machine vierge.
4. **CI** rejoue les mêmes scripts pour éviter la phrase « chez moi ça marche ».
5. **Merge** seulement quand tout est vert.

---

## 3. Lint vs Format : qui fait quoi ?
| Outil | Action | Quand l’utiliser ? |
| --- | --- | --- |
| **ESLint** | Signale variables inutilisées, imports manquants, règles métiers… | Après avoir modifié du code, avant de committer.
| **Prettier** | Ajuste les espaces, guillemets, retours à la ligne. | Dès que tu veux nettoyer le style (`npm run format`).

> Astuce : ajoute un script `"format:fix": "prettier -w ."` si tu veux tout corriger automatiquement.

---

## 4. Exemple de test Vitest
```js
import { describe, it, expect } from 'vitest';
import { sum } from '../src/sum.js';

describe('sum', () => {
  it('adds two numbers', () => {
    expect(sum(2, 3)).toBe(5);
  });
});
```

- Fichiers de test : `tests/*.test.js`.
- Commande de base : `npm run test` (mode batch) ou `npx vitest` (watch interactif).

---

## 5. Pourquoi `--if-present` en CI ?
Le workflow CI utilise `npm run <script> --if-present` pour exécuter un script **uniquement s’il existe** dans `package.json`. Résultat :
- un template peut être cloné sans erreur même si `typecheck` ou `build` ne sont pas encore définis ;
- chaque projet active la brique dont il a besoin, sans modifier la CI.

---

## 6. Routine « avant PR »
- [ ] Mettre à jour ta branche depuis `main`.
- [ ] Vérifier `npm run lint` (et corriger/auto-fixer si nécessaire).
- [ ] Vérifier `npm run test` (et `npm run typecheck`/`npm run build` si le projet les fournit).
- [ ] S’assurer que la documentation et le changelog sont à jour.
- [ ] Relire la PR comme si tu étais reviewer (noms, logs, code mort…).

---

## 7. Dépannage rapide
| Symptôme | Piste rapide |
| --- | --- |
| `npm install` échoue | Vérifie ta version de Node (utilise `nvm use 20`). |
| ESLint hurle sur un import inexistant | Assure-toi que le fichier est bien renommé/exporté. |
| Vitest ne trouve pas un module | Chemin relatif incorrect (`../` vs `./`). |
| Build casse uniquement en CI | La CI part d’un dossier vide : ajoute les fichiers requis ou les variables d’environnement. |
| Différents résultats entre local et CI | Supprime `node_modules`, relance `npm install` pour reproduire l’environnement propre. |

---

## 8. Glossaire express
- **CI** : Continuous Integration, automatisation des vérifications à chaque commit.
- **Lockfile** : `package-lock.json`, photo précise des versions installées.
- **Runner** : machine virtuelle qui exécute ton workflow GitHub Actions.
- **Script npm** : commande déclarée dans `package.json` (utilisable via `npm run <nom>`).

---

## 9. Check-list « nouveau projet »
- [ ] Cloner le template et mettre à jour le `README` avec le contexte projet.
- [ ] Personnaliser `package.json` (nom, scripts, dépendances).
- [ ] Exécuter l’action **Generate lockfile (PR)** pour créer `package-lock.json`.
- [ ] Configurer les secrets nécessaires pour la CI (si build déploiement, etc.).
- [ ] Activer les protections de branche (`main` protégée, revue obligatoire).
- [ ] Compléter la documentation métier/technique (docs/, Notion, etc.).

---

Garde ce playbook à portée de main : il évite la dette technique et rassure les reviewers. 🙌
