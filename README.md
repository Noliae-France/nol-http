# nol.http

Serveur et briques HTTP en pur [Nolc](https://github.com/Noliae-France/nolc), sans dépendance lourde.

> **État : fondation (v0.1).** Les primitives ci-dessous sont implémentées et testées. La feuille de route liste la suite. Construit lot par lot, chaque étape avec CI verte.

## Installation

```toml
[dependances]
"nol-http" = { git = "https://github.com/Noliae-France/nol-http" }
```

## Licence

MIT © 2026 Bastien LANGUEDOC.

## Livré (v0.1)
`methode_valide`, `chemin_normalise`, `chemin_correspond`, `statut_texte`.

## Feuille de route
- Parseur HTTP/1.1 (requêtes/réponses typées) puis **HTTP/2** (framing, HPACK, multiplexing, flow control)
- Routeur typé (`/u/:id`), middlewares pré/post
- Streaming / SSE / chunked, fichiers statiques
- Limites de requêtes, timeouts réseau réels, backpressure, **arrêt gracieux**
