# Site personnel — Jonathan Cazabonne

Site vitrine de recherche (mycologie / conservation fongique), construit comme une page HTML unique, autonome, sans dépendance ni backend.

## Structure

```
.
├── index.html   ← tout le site (HTML + CSS + JS + contenu) — le seul fichier qui compte
├── README.md    ← ce fichier
└── NOTES.md     ← notes de conception et historique des décisions, migrées depuis le projet Claude
```

Tout le contenu (bio, publications, communications, frise de conservation, zones d'étude, contact) est stocké **à l'intérieur même de `index.html`**, dans un bloc de données JavaScript en haut du script. Il n'y a pas de base de données, pas de CMS, pas d'étape de build : ouvrir le fichier dans un navigateur suffit pour voir le site tel qu'il sera en ligne.

## Éditer le contenu avec Claude Code

Ouvre ce dossier avec Claude Code (`claude` dans le terminal, depuis ce dossier) et décris ce que tu veux changer en langage naturel : "change mon email de contact pour X", "ajoute cette publication à la page Publications", "renomme la première study area en 'Forêt boréale — Abitibi'", etc. Claude Code peut modifier `index.html` directement.

Le bloc de données à modifier s'appelle `DEFAULT_DATA` dans le `<script>` du fichier — c'est là que vivent tous les textes, dates, liens et entrées de listes.

## Le bouton crayon (édition dans le navigateur)

Le site garde son bouton d'édition intégré (le crayon en bas à droite). Sur claude.ai, ce bouton republiait la page automatiquement ; **ici, en dehors de Claude, il télécharge un nouveau `index.html` mis à jour** avec tes changements. Le flux devient :

1. Ouvre `index.html` dans ton navigateur, clique sur le crayon, modifie ce que tu veux.
2. Clique "Save changes" → un fichier `index.html` à jour est téléchargé.
3. Remplace l'ancien `index.html` du projet par celui-ci.
4. Commit + push (demande à Claude Code de le faire, ou fais-le toi-même) pour mettre le site en ligne à jour.

C'est un aller-retour manuel plutôt qu'une sauvegarde automatique, mais ça marche sans aucun serveur ni compte à gérer.

## Mettre le site en ligne

Pas encore configuré — à décider quand tu seras prêt. Les options les plus simples pour un site 100% statique comme celui-ci :

- **GitHub Pages** (gratuit) : pousse ce dossier vers un dépôt GitHub, active Pages dans les réglages du dépôt, et `index.html` devient accessible à `https://<ton-compte>.github.io/<nom-du-dépôt>/`.
- **Netlify** ou **Vercel** (gratuits) : glisser-déposer le dossier sur leur interface, ou connecter le dépôt GitHub — déploiement automatique à chaque push.

Dis-le à Claude Code (ou reviens ici) quand tu veux qu'on mette ça en place, il pourra initialiser le dépôt git et préparer le déploiement.

## Limite à garder en tête

Les photos (Study Areas, photo de contact) sont compressées automatiquement à l'upload puis stockées directement dans `index.html` (encodage base64) — pas de dossier `images/` séparé. C'est volontaire (site 100% autonome, un seul fichier), mais ça veut dire que `index.html` grossit avec chaque photo ajoutée. Avec la compression en place, ça reste largement gérable pour quelques dizaines de photos ; au-delà, il faudrait passer à un vrai dossier d'images séparé.
