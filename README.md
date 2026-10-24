# Stacktrace Lens

Colle un log ou une stack trace **Laravel, Node.js ou Python** : l'outil détecte le format, regroupe les erreurs identiques, met en avant les lignes de **ton** code et replie le bruit (`vendor/`, `node_modules/`, `site-packages/`).

**[Voir la démo](https://kevinadeossi.github.io/stacktrace-lens/)** : clique sur un exemple pour l'essayer sans log.

![Capture d'écran](./screenshot.png)

## Le problème

Des milliers de lignes de log, des stack traces dont 90 % pointent dans des dépendances, et la même erreur répétée des centaines de fois. Trouver la vraie cause prend plus de temps que la corriger.

## Ce que fait l'outil

- Détection automatique du format (ou choix manuel)
- Regroupe les erreurs identiques avec un compteur (IDs, nombres, UUID et emails normalisés)
- Affiche l'origine de l'erreur dans ton code
- Replie les frames de dépendances et les frames internes
- Filtre par niveau et par texte, trie par fréquence, récence ou gravité
- Copie un résumé propre pour un ticket

## Formats supportés

| Format | Reconnu | Dépendances repliées |
|---|---|---|
| Laravel / Monolog | `[date] env.LEVEL: message` + frames `#0` | `vendor/` |
| Node.js | `Error: ...` + lignes `at fn (fichier:ligne:col)` | `node_modules/`, `node:internal` |
| Python | blocs `Traceback (most recent call last):` | `site-packages/`, stdlib |

## Ajouter un format

Un format = un objet dans le tableau `PARSERS` :

```js
{ id: 'monformat', name: 'Mon format',
  detect: texte => /regex/.test(texte),
  parse: texte => [{ date, level, msg, frames: [{ file, line, call, vendor }] }] }
```

## Structure

- `index.html` : l'outil (HTML, CSS, JS)
- `hints.js` : les règles de pistes rapides

## Aide à la résolution

- **Pistes rapides** : des règles intégrées (`hints.js`) proposent instantanément une cause probable et un correctif pour les erreurs courantes. Aucune IA, aucun réseau.
- **Analyser avec Claude** (optionnel) : un bouton par erreur envoie ce seul groupe d'erreur à l'API Anthropic et affiche la réponse en direct. Il faut sa propre clé API, saisie dans la page.

## Confidentialité

L'analyse des logs est 100 % locale. Seul le bouton « Analyser avec Claude » envoie des données : le message et les frames d'**une** erreur, avec emails, tokens et mots de passe masqués, et le contenu exact est affiché avant la réponse. Ta clé API reste dans ton navigateur (jamais dans le code). Ne soumets pas de logs contenant des données sensibles que tu n'as pas le droit d'envoyer à un tiers.

## Limites

- Formats texte standards uniquement (pas de logs JSON pour l'instant)
- Très gros fichiers (plusieurs dizaines de Mo) : tout est analysé en mémoire

## Stack

HTML, CSS et JavaScript vanilla. Pas de build, pas de framework.

## Licence

MIT
