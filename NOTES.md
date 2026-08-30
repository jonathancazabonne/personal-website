# Site internet de Jonathan — état d'avancement

**Migré vers Claude Code le 30/08/2026.** Ce fichier est la copie exacte des notes tenues dans le projet Claude "Site internet" pendant la phase de conception sur claude.ai. L'artefact original (lecture seule désormais comme référence/sauvegarde) reste accessible ici :
**Artifact d'origine :** https://claude.ai/code/artifact/ad14f4e0-8567-4e6b-bc08-ec582927ccd9

À partir de maintenant, `index.html` dans ce dossier est la version de travail — c'est lui qu'il faut éditer et déployer, pas l'artefact.

---

**Artifact publié :** https://claude.ai/code/artifact/ad14f4e0-8567-4e6b-bc08-ec582927ccd9
(privé, éditable via le bouton crayon en bas à droite — sauvegarde en republiant la page elle-même)

## Décisions prises (29/08/2026)
- **Langue :** anglais uniquement.
- **Sections :** Home (hero dynamique + à propos), Publications, Communication & vulgarisation, Fungal Conservation (frise chronologique), Study Areas (carrousels photo), Contact (page à part) — chacune avec bouton d'édition.
- **Police :** Arial / Helvetica partout sur le site (pas de Google Fonts — Jonathan voulait une police simple et très lisible, "pas la police que Claude utilise").
- **Contenu :** provisoire (placeholders) sauf la frise Fungal Conservation, qui contient les 94 vrais jalons du fichier Excel fourni par Jonathan.
- **Édition autonome :** bouton crayon flottant → mode édition → "Save changes" republie la page elle-même (capability `artifact`). Lecture seule pour un visiteur non-propriétaire.

## Design
- Palette : fond papier sauge clair `#EFF1E8` / encre `#171C15` / accent mousse `#3F5C3F` / accent secondaire ocre `#A8813C` (inversés en mode sombre).
- Typographie : Arial/Helvetica (var(--font)) partout, gras pour les titres, italique conservé pour le mot rotatif du hero.
- Mise en page : colonne unique asymétrique alignée à gauche, fine ligne + points évoquant un réseau mycélien.
- Navigation par hash sur une seule page HTML : `#top`, `#publications`, `#communications`, `#conservation`, `#study-areas`, `#contact`. Nav responsive (wrap sur mobile, 6 items).

## Page Fungal Conservation
- Frise chronologique **en zigzag** : à partir de 700px de large, les points alternent à gauche/droite d'une ligne centrale avec un petit connecteur horizontal ; en dessous de 700px, colonne unique classique pour rester lisible sur mobile.
- Classée par année croissante (1971 → 2026), défilement normal de la page.
- Chaque point affiche année + résumé (titre tronqué à 2 lignes) ; au survol (desktop) ou au tap/clic (mobile/clavier), un panneau se déplie avec le titre complet, la portée géographique et le(s) lien(s) source.
- Champ "Notes" du fichier Excel volontairement PAS affiché publiquement — gardé dans les données pour référence. À confirmer avec Jonathan si certaines devraient être publiques.
- **Import Google Sheets (mode édition)** : Jonathan copie les lignes de son Google Sheet et les colle dans une zone de texte → bouton "Replace timeline with pasted data" → "Save changes" (un fetch direct était bloqué par la sécurité de la page publiée sur claude.ai — hors de claude.ai ça reste la méthode la plus simple, un vrai fetch cross-origin vers Google Sheets nécessiterait une clé API et poserait des soucis de CORS).

## Page Study Areas (29/08/2026)
- Un carrousel photo horizontal **par zone d'étude** (`studyAreas` : nom, description courte, liste de photos), avec flèches ‹ › pour défiler (masquées sur mobile où le scroll tactile suffit).
- Upload de photos en mode édition : bouton "Add photos" (multi-fichiers) par zone → compression côté client (`FileReader` → `canvas` → JPEG qualité 0.78, largeur max 1400px) → stocké en `data:` URI directement dans le document (pas de backend, pas de dossier `images/` séparé — un seul fichier autonome).
- Bouton "+ Add study area" pour créer de nouvelles zones ; bouton × pour en supprimer une, et × sur chaque vignette pour retirer une photo individuelle.
- Les légendes de photo (`caption`) existent dans le modèle de données mais n'ont pas encore d'UI d'édition dédiée — actuellement toujours vides. À ajouter si Jonathan veut légender ses photos.

## Page Contact (page à part, 29/08/2026)
- Photo de profil auto-uploadable (mêmes compression/encodage que Study Areas), avec bouton pour la remplacer/retirer.
- Adresse (zone de texte multi-lignes), email, et une liste de liens à icônes (`links`) éditable : chaque lien a un type d'icône (menu déroulant : Google Scholar, LinkedIn, ORCID, ResearchGate, institution/bâtiment, ou lien générique), un libellé et une URL. Boutons "+ Add link" / × pour ajouter/retirer des liens librement.
- Icônes dessinées à la main en SVG inline (pas de reproduction de logos de marque, pas de police d'icônes externe).
- Contenu actuel : email + 4 liens (Google Scholar, LinkedIn, ORCID, "Centre for Forest Research (CFR)") — placeholders à remplacer par les vraies URLs de Jonathan, y compris ResearchGate s'il le souhaite (déjà proposé comme option d'icône).

## Bug corrigé (29/08/2026)
- La fonction qui reconstruit le document complet à chaque sauvegarde oubliait de redonner l'attribut `id="app-script"` à la balise `<script>` réintégrée. Conséquence : la **toute première** sauvegarde fonctionnait, mais **toute sauvegarde suivante** échouait silencieusement. Corrigé — testé avec plusieurs sauvegardes consécutives, aucune erreur.

## Adaptation faite pour la migration (30/08/2026)
- Le site utilisait la capability `artifact` propre à claude.ai (`window.claude.use("artifact")`) pour republier la page en un clic. Cette capability n'existe pas en dehors de claude.ai.
- **Remplacement :** le bouton "Save changes" télécharge désormais un `index.html` mis à jour (via `Blob` + lien de téléchargement, une API standard du navigateur, aucune dépendance externe). Le bouton crayon reste visible en permanence (avant, il se cachait si aucune capability n'était détectée).
- Flux d'édition résultant : ouvrir `index.html` → crayon → modifier → "Save changes" → remplacer le fichier dans le projet → commit/push pour mettre à jour le site en ligne.
- Testé : navigation sur les 6 pages, édition/annulation sur chacune, deux sauvegardes consécutives (le fichier téléchargé peut être réédité et re-sauvegardé sans casser), zéro erreur console.

## Limite technique à garder en tête
- Pas de stockage externe : chaque photo uploadée (contact ou study areas) est encodée en base64 et vit directement dans `index.html`. Avec la compression appliquée, chaque photo pèse grosso modo 100–300 Ko selon son contenu — largement de quoi mettre quelques dizaines de photos, mais si Jonathan veut un jour une vraie photothèque de terrain (centaines de photos), il faudra revoir l'approche (dossier `images/` séparé, ou hébergement externe).

## Contenu à corriger avec Jonathan (placeholders restants)
- Home : titre/rôle, paragraphe "About the research" (placeholder explicite).
- Contact : email à confirmer (jcazabonne@mycosphaera.com), URLs réelles des liens (Scholar/LinkedIn/ORCID/CFR), photo de profil à uploader.
- Publications (3 exemples fictifs) et communications (4 exemples fictifs) à remplacer par les vraies entrées.
- Study Areas : 2 zones d'exemple ("Boreal forest", "Temperate forest") sans photos — à renommer et à illustrer avec les vraies photos de terrain de Jonathan.
- Fungal Conservation : contenu réel déjà en place ; à tenir à jour via le copier-coller depuis Google Sheets.

## Prochaines étapes possibles
1. Connecter un dossier du Mac de Jonathan à une session Claude pour terminer l'installation locale (git init, premier commit).
2. Décider de l'hébergement (GitHub Pages, Netlify ou Vercel — en attente).
3. Recueillir le contenu réel du reste du site (titre, affiliation, bio, vraies publications/communications, URLs de contact).
4. Jonathan uploade ses vraies photos de terrain dans Study Areas et sa photo dans Contact.
5. Décider si les "Notes" internes de la frise doivent apparaître publiquement (actuellement non).
6. Ajouter une UI de légende pour les photos de Study Areas si souhaité.
