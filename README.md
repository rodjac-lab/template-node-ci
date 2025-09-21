# template-node-ci
Template pour ma CI
# Template Node CI

Starter minimal pour projets Node/JS avec CI GitHub Actions, ESLint, Prettier et tests Vitest.

## Prise en main

```bash
npm ci
npm run lint
npm run test

template-node-ci/
├─ .github/
│  └─ workflows/
│     └─ ci.yml
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

