# Central Renovate Configuration Preset

Central Renovate shared configuration preset for the **Antigravity Squadron** repository fleet.

## Usage

In any fleet repository, create a minimal `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>jhathcock-sys/renovate-config"
  ]
}
```

Because this repository is public, `github>jhathcock-sys/renovate-config` resolves automatically across both public and private repositories without requiring custom authentication tokens.

## Preset Features & Rules

1. **Noise-Free Schedule & Timezone:**
   - Evaluated under `America/New_York`.
   - Runs off-peak: `before 5am on monday`.
2. **Supply Chain Defense:**
   - `minimumReleaseAge: "3 days"` cooling-off buffer before PR creation to guard against zero-day compromised upstream releases.
   - `prConcurrentLimit: 5` to prevent triage storms.
3. **5–8 Logical Grouping Rules:**
   - **Vite, Rollup & Vitest ecosystem:** `vite*`, `@vitejs/*`, `rollup*`, `@rollup/*`, `vitest*`, `@vitest/*`
   - **React & TypeScript core:** `react*`, `@types/react*`, `typescript`
   - **Styling & UI:** `tailwindcss*`, `postcss*`, `lucide-react`, `@radix-ui/*`
   - **Firebase & Google Cloud:** `firebase*`, `@google/*`, `@google-cloud/*`
   - **Linting & QA Tooling:** `oxlint*`, `eslint*`, `@typescript-eslint/*`, `@playwright/*`, `husky`, `secretlint*`
   - **Python & Container Digests:** `dockerfile`, `docker-compose`, `pip_requirements`, `pep621`
4. **Strict Automerge Policy:**
   - Safe automerge enabled **ONLY** for `patch`, `pin`, and `digest` updates after passing CI checks.
   - Automerge disabled for `minor` updates.
   - Automerge explicitly disabled for `major` updates; tagged with labels `deps: major` and `breaking`.

## Validation

Validate configuration files locally:

```bash
python3 -m json.tool default.json
NPM_CONFIG_CACHE=/mnt/optane/npm-cache npx --yes --package renovate renovate-config-validator --strict --no-global default.json
NPM_CONFIG_CACHE=/mnt/optane/npm-cache npx --yes --package renovate renovate-config-validator --strict --no-global renovate.json5
```
