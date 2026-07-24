# nol.http

Serveur et briques HTTP/1.1 en pur [Nolc](https://noliae-nolc.s3.gra.io.cloud.ovh.net/nolc-latest-linux-x86_64.tar.gz), sans dépendance.

## Installation

```toml
[dependances]
"nol-http" = { git = "https://github.com/Noliae-France/nol-http" }
```

## Serveur HTTP/1.1 (v0.2)

Un serveur concurrent complet — voir [`examples/serveur.nol`](examples/serveur.nol), **testé en intégration à chaque CI** (le serveur est lancé et sollicité pour de vrai) :

- **Concurrence réelle** : un thread (`spawn`) par connexion.
- **Timeouts** : lecture bornée par connexion (`tcp_set_timeout`).
- **Limites** : taille de requête plafonnée d'après `Content-Length` (réponse **413**).
- **Backpressure** : écriture socket bornée (envoi bouclé jusqu'à complétion).
- **Arrêt gracieux** : `GET /stop` signale l'arrêt via un canal ; la boucle d'acceptation cesse et **draine les connexions en vol** (`groupe_attend`).

## Bibliothèque (briques testées à l'unité)

- **Requête** : `parse_requete(brut) -> RequeteHttp` (méthode, cible, chemin, query, version, corps), `entete_de(brut, clé)`, `query_param`, `depasse_taille`.
- **Réponse** : `reponse(code, content_type, corps)`, `reponse_texte`, `reponse_html`, `reponse_json`, `statut_texte` (codes 2xx–5xx).
- **Streaming** : `entetes_flux(code, type)`, `chunk(données)`, `chunk_fin()` (Transfer-Encoding: chunked).
- **Routage** : `methode_valide`, `chemin_normalise`, `chemin_correspond`.

```nol
let r = parse_requete(brut)
if r.chemin == "/" { reponse_html(200, "<h1>Bonjour</h1>") }
```

## Feuille de route
- Routeur à motifs (`/u/:id`), middlewares pré/post
- Fichiers statiques, HTTP/2
- Accumulation de corps > 64 Ko (lecture en boucle sur `Content-Length`)

## Licence

MIT © 2026 Bastien LANGUEDOC.
