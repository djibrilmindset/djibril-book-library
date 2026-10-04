# Patterns Reset Ultra — sauvegarde des corpus (04/10/2026)

Deux fichiers gzip = **copie de sauvegarde** des corpus de patterns qui vivent dans
`~/.hermes/state/` (Hermes). Ils sont ici pour ne jamais les perdre ; la source active
reste `~/.hermes/state/`.

| Fichier | Source active | SHA-256 (source brute) | Lignes | Rôle |
|---|---|---|---|---|
| `patterns_index.jsonl.gz` (4,3 Mo) | `~/.hermes/state/patterns_index.jsonl` | `3781d969c8b6953f4082b2d5baa31761c1613716edb8e531e780602bb4b7cc97` | 16 505 (après dédup 04/10 : 16 842 → 16 505, 337 doublons d'ID, backup `.pre-dedup-0410`) | index complet servi par `pattern_select.py` / `jev_copilote.py` |
| `patterns_livres.jsonl.gz` (2,8 Mo) | `~/.hermes/state/patterns_livres.jsonl` | `1d53d9290136907c38d6e7fc8fbb758a50101fb326aa720e9a17c2f5f969bc17` | 7 551 (7 525 ID uniques) | corpus des livres scannés (source du sidecar app) |

**Chaîne complète (côté app `reset-ultra-os`)** :
`patterns_livres.jsonl` → `node scripts/rafraichir-patterns-livres.mjs` (SEUL producteur)
→ `src/lib/onboarding-vendor/bibliotheque/patterns-livres-reset-ultra.json` (7 550 motifs /
284 livres) → `patternsMaisonPour()` → `/api/copilote` + analyseurs.
Verrou : `scripts/__tests__/patterns-livres-sidecar.test.ts` (drift + contrat de forme).

Rafraîchir ces sauvegardes :
`gzip -c ~/.hermes/state/patterns_index.jsonl > patterns/patterns_index.jsonl.gz`
et idem pour `patterns_livres.jsonl`, puis mettre à jour les SHA-256 ci-dessus.
