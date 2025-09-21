# Template Node CI

[![CI](https://github.com/OWNER/REPO/actions/workflows/ci.yml/badge.svg)](https://github.com/OWNER/REPO/actions/workflows/ci.yml)

Starter minimal pour projets Node/JS avec CI GitHub Actions, ESLint, Prettier et tests Vitest.

## Scripts npm

| Script | Description |
| --- | --- |
| `npm run lint` | Lance ESLint sur tout le projet. |
| `npm run format` | Vérifie que Prettier laisserait le formatage inchangé. |
| `npm run test` | Exécute la suite de tests Vitest en mode batch. |
| `npm run typecheck` | _(Optionnel)_ Lance un vérificateur de types (ex. `tsc`, `tsc --noEmit`, `tsd`). |
| `npm run build` | _(Optionnel)_ Construit l’application ou la librairie. |

> Ajoute les scripts optionnels quand ton projet en a besoin : la CI les détecte automatiquement grâce à `--if-present`.

## Documentation utile
- [Playbook CI + Tests + Lint](docs/Playbook-CI-Tests.md)

## Comment générer le lockfile initial ?
1. Aller dans **GitHub → Actions → Generate lockfile (PR)**.
2. Cliquer sur **Run workflow** et valider.
3. Attendre que la PR `ci/add-lockfile` soit ouverte.
4. Revue rapide puis merge : `package-lock.json` devient la référence officielle pour `npm ci`.

## Structure
```
template-node-ci/
├─ .github/
│  ├─ pull_request_template.md
│  └─ workflows/
│     ├─ ci.yml
│     └─ generate-lockfile.yml
├─ docs/
│  └─ Playbook-CI-Tests.md
├─ src/
│  └─ sum.js
├─ tests/
│  └─ sum.test.js
├─ eslint.config.js
├─ prettier.config.cjs
├─ package.json
└─ README.md
```
