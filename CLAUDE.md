# CLAUDE.md — Z'amours and Go

Contexte pour Claude (et pour qui reprend le projet). Le mode d'emploi du jeu est dans `README.md`.

## En deux phrases

Pupitre d'un jeu de soirée entre collègues, façon « Mr & Mrs » : des duos doivent deviner les
réponses l'un de l'autre, sur un seul écran projeté que l'animateur pilote au clavier. Créé par
Ludovic Bonnefoi pour le happiness Evalandgo de juillet 2026, transmis en septembre 2026 à une
collègue qui organise les sessions suivantes, **sans être tech** : chaque explication qui lui est
destinée doit rester lisible par quelqu'un qui ne code pas.

## Règles du projet

- **Un seul fichier, aucune dépendance, aucun build.** `zamours.html` s'ouvre en double-clic et
  marche hors connexion. Pas de npm, pas de CDN, pas de framework. Toute nouvelle fonctionnalité
  tient dans ce fichier.
- **Un seul écran, vu par toute la salle.** Rien de ce qui s'affiche ne doit s'adresser à
  l'animateur seul (pas de réponse attendue, pas de « note pour toi »).
- **Le contenu s'écrit au neutre.** La même question sert pour toutes les personnes : « en rentrant
  du boulot » plutôt que « quand il rentre ». Options de QCM sans sujet : « Tourne autour toute la
  journée… ». Seuls les Jokers, écrits pour un duo précis, peuvent être genrés.
- **Tout texte venant du pack passe par `esc()`** avant d'entrer dans le HTML : un pack peut venir
  de quelqu'un d'autre.
- **Pas de `confirm()` ni d'`alert()`** : un navigateur réglé pour bloquer les boîtes de dialogue les
  rend muettes. Une action destructrice se confirme en deux clics avec `confirme(cle)` et
  `libelle(cle, texte)`.
- **Commits en français**, préfixés `feat:`, `fix:`, `content:` (texte du pack) ou `chore:`. On ne
  commite qu'après avoir testé (auto-test au vert et essai à l'écran) et avec l'accord de la
  personne qui maintient le jeu.

## Tester

1. **Auto-test** : ouvrir `zamours.html#autotest`. La page joue une partie complète avec le pack
   chargé, sans rien sauvegarder, puis affiche en bas à gauche ce qui casse. Elle vérifie :
   - qu'un pack vide est bloqué ;
   - que les scores de la Manche 1 correspondent aux cases cochées ;
   - que la mention « options masquées » n'apparaît que sur un QCM ;
   - que la Manche 2 attribue le bon total à chaque duo ;
   - que la finale désigne le bon vainqueur ;
   - qu'aucun `undefined`, `NaN` ou `null` ne s'affiche à aucune étape.

   **À relancer après toute modification du moteur.** Le résultat est aussi exposé dans
   `window.resultatAutotest` (tableau vide = OK). Si la partie chargée n'est pas encore jouable
   (une partie en préparation, sans duos), le test joue la partie de juillet 2026 et le signale.
2. **Depuis Claude**, les outils du navigateur n'agissent pas sur une page `file://`. Il faut servir
   le dossier :
   ```bash
   python3 -m http.server 8765 --bind 127.0.0.1
   ```
   puis ouvrir `http://127.0.0.1:8765/zamours.html?v=1#autotest` et lire `window.resultatAutotest`.
   **Changer `?v=` à chaque rechargement** : le navigateur sert sinon l'ancienne version depuis son
   cache, et le test passe au vert sur du code qui n'existe plus.
3. **Un test ne doit pas réutiliser le code qu'il vérifie.** Exemple vécu : comparer l'affichage du
   QCM à `aDesOptions()` laissait passer un `aDesOptions()` cassé. On compare à `q.opts.length`.
   Pour s'assurer qu'une vérification sert à quelque chose, on réintroduit le bug dans une copie
   jetable et on regarde l'auto-test échouer.
4. **L'œil reste indispensable** pour les animations et la mise en page. Une capture prise pendant
   une animation d'entrée montre un écran vide : attendre 3 s ou recapturer.
5. **Trappes de secours** : `#reset` efface la partie en cours (pas le pack) ;
   `localStorage.clear()` efface tout.

## Architecture de `zamours.html`

Environ 4 600 lignes. Ne pas le lire d'un bloc : repérer les sections avec
`grep -n '/\* ====' zamours.html`. Dans l'ordre :

| Section | Contenu |
|---|---|
| `<style>` (≈1 700 premières lignes) | Habillage pop-art : tokens dans `:root`, puis un bloc par écran et par animation |
| `CONTENU` | `PACK_DEFAUT` (la partie complète de juillet 2026), `PACK_VIDE`, et `MODELES`, les trois points de départ d'une partie : `nouvelle` (les questions de juillet sans les duos ni leurs Jokers), `vide`, `demo` (juillet complet) |
| `PACK ACTIF ET VUES DÉRIVÉES` | `appliquerPack()`, `normaliserPack()` (remplit ce qui manque, assainit les ids), `validerPack()` (liste des erreurs bloquantes) |
| `ÉTAT` | `S`, l'état de partie ; `freshState()`, `hydrate()` (recolle un état sauvegardé au pack actuel), `scoresM1()` et `scoresM2()` |
| `TIMER`, `HELPERS`, `RENDU` | Chrono, `esc()`, `rank()` ; `render()` redessine tout l'écran à chaque action |
| Animations | Révélation de question, faux départ du Joker, splash de manche, annonce de duo, sponsor, cœur brisé, victoire |
| `SCREENS.*` | Un objet `{ top, stage, bottom }` par écran, qui renvoie du HTML |
| `ACTIONS` | `act(action, data)` : un seul `switch`, déclenché par les attributs `data-act` des boutons |
| `CLAVIER` | → ← Espace R S, coupés sur les écrans `edit`, `pack` et `choix` |
| `ÉDITEUR DE PACK` | Onglets, écriture des champs, actions de structure, import et export JSON |
| `AUTO-TEST`, `BOOT` | `autotest()`, puis le chargement : pack d'abord, état de partie ensuite |

### Écrans (`S.screen`)

`choix` (premier lancement, puis bouton « Nouvelle partie ») → `title` → `m1rules` → `m1` → `m1res` →
`m2rules` → `m2` → `m2res` → `m3rules` → `m3` → `end`. Une partie créée sans duos ouvre directement
l'éditeur, sur l'onglet des duos. À côté : `edit` (éditeur), `pack` (récapitulatif et erreurs), `resume` et
`resume3` (reprendre directement en Manche 2 ou 3 quand la soirée est coupée en deux).

### Déroulé du jeu (tel que codé)

- **Manche 1, Qualifications** :
  - toutes les questions pour tous les duos ;
  - une « cible » par duo écrit sa vraie réponse, l'autre membre la devine ;
  - la cible alterne à chaque question (`cibleSide(d, i)`, point de départ tiré au sort dans `S.cible`) ;
  - l'animateur désigne les 2 duos qui sortent : ils forment le Jury du Fail.
- **Manche 2, Joker et Catégories** :
  - les scores repartent de zéro ;
  - tour 0 : le Joker de chaque duo, 3 points ;
  - tours 1 à `m2Tours − 1` : le duo choisit une catégorie, une question y est tirée au hasard puis retirée de la pioche, 2 points ;
  - l'animateur désigne 2 finalistes.
- **Manche 3, Longueur d'onde** :
  - les deux membres écrivent en même temps ;
  - une réponse différente coûte un cœur (`m3Coeurs` cœurs au départ) ;
  - en cas d'égalité, mort subite.

### Clés de l'état `S`

- `m1marks[indexQuestion][idDuo] = 1` : case cochée.
- `m2marks["<idDuo>-j"]` pour le Joker et `m2marks["<idDuo>-r<tour>"]` pour une catégorie.
  `m2cat` et `m2drawn` utilisent les mêmes clés `-r<tour>` et retiennent la catégorie et la question tirées.
- `m2used[idCategorie]` : questions déjà tirées.
- `m3marks["<indexQuestion>|<idDuo>"]` : cœur perdu sur cette question.

### Pièges connus — à respecter

- **Les scores ne sont jamais stockés**, ils se recalculent depuis les cases cochées. Un compteur
  tenu à côté a déjà produit des scores à `NaN` puis négatifs.
- **« A des options » se teste avec `aDesOptions(o)`**, jamais avec `!!q.opts` : le normaliseur
  transforme « pas d'options » en `[]`, qui vaut vrai en JavaScript. Ce piège transformait toutes
  les questions en QCM.
- **Un texte qui cite les réglages du pack se calcule à l'affichage**, jamais au chargement du
  script : le pack n'est appliqué qu'au `BOOT` et change dans l'éditeur. Voir `regles()`, qui
  affichait `undefined` quand c'était une constante.
- **Toute contrainte de jouabilité va dans `validerPack()`**. `jouable()` s'en sert pour bloquer
  « Commencer » et les reprises, et l'éditeur l'affiche dans son bandeau.
- **Éditeur : aucun re-rendu pendant la frappe**, sinon le curseur saute. Le texte s'écrit dans le
  pack à l'événement `input` ; seules les actions de structure (ajouter, supprimer, déplacer)
  redessinent l'écran, puis rejouent `hydrate(S)`, car une partie en cours peut citer un duo supprimé.
- **Changer la forme de `S`** (pas juste son contenu) : incrémenter `KEY` (`zamours-console-v4`)
  pour invalider les parties sauvegardées. Changer la forme du pack : faire évoluer
  `normaliserPack()`, pour que les anciens fichiers `.json` restent lisibles.
- **Le repère du prénom est `...` (trois points) ou `…` (U+2026)**, découpé par `qText()`. Les
  deux valent pour l'ancien contenu comme pour le nouveau. Contrepartie : trois points ne peuvent
  plus servir de vrais points de suspension dans une question (le faux départ d'un Joker, lui, ne
  passe pas par `qText()`).
- **Animations** : chaque `maybeX()` garde une clé pour ne pas rejouer sur un simple re-rendu.
  `nettoyerEffets()` remet tout à zéro : l'appeler après tout changement de partie ou de pack.
- **Polices** : Impact et Brush Script sont des polices Windows. Sous Linux ou macOS, la page
  retombe sur des polices génériques. C'est normal, ce n'est pas un bug d'affichage.

## Stockage (état actuel)

- **Le navigateur garde la partie** dans son `localStorage`, sous trois clés :
  - `zamours-pack-v1` : la partie préparée ;
  - `zamours-console-v4` : la partie en cours de jeu ;
  - `zamours-fichier-v1` : le dernier fichier ouvert ou enregistré, et si la partie a changé depuis.

  Ce stockage est lié au navigateur, et sous Firefox à l'emplacement du fichier : vider les données
  du navigateur, changer de PC ou déplacer le fichier fait perdre la partie. **Seul un fichier
  `.json` la conserve vraiment.**
- **« 💾 Enregistrer »** (`exporterPack()`) télécharge la partie en `.json`. Le navigateur renomme
  chaque nouvel enregistrement « (1) », « (2) »…
- **« 📂 Ouvrir une partie »** (`importerPack()`, ou un fichier glissé sur la page) passe par
  `ouvrirFichier()`, qui :
  - refuse tout JSON qui n'a pas `duos` et `manche1` ;
  - télécharge une copie de sécurité si la partie remplacée avait des modifications jamais
    enregistrées ;
  - ne prend pas un fichier glissé en plein jeu.
- **L'éditeur et l'écran récapitulatif affichent toujours** où en est l'enregistrement
  (`etatSauvegarde()`). Fermer l'onglet sur des modifications non enregistrées déclenche l'alerte du
  navigateur (`beforeunload`).
- **Toute vraie modification dans l'éditeur doit appeler `marquerModifie()`**. Changer d'onglet ne
  compte pas.

## Décisions prises (24/09/2026)

- **Stockage hybride** : un HTML moteur unique, et une session par fichier JSON. Le JSON est la
  source de vérité : c'est lui qu'on s'échange et que Claude édite pour le contenu, sans toucher au
  moteur. En plus, un bouton pour exporter une version autonome (un seul HTML avec le pack
  embarqué), pour le jour J ou pour l'envoyer à quelqu'un.
- **Les manches deviennent des modules dès la phase 2**, pour pouvoir en ajouter.
- **Contenu au neutre**, sans champ de pronom par personne.
- Dépôt privé `ChipsAKALudo/zamours-and-go`.

## Reste à faire

### Phase 2 — généraliser (pas commencée)

Ordre conseillé : l'auto-test protège chaque étape, on l'étend au fur et à mesure.

1. **Paramètres de manche dans le pack.** Aujourd'hui en dur :
   - 2 éliminés en Manche 1 et 2 finalistes (`act()` : `elim`, `final` ; `SCREENS.m1res`,
     `SCREENS.m2res` ; le texte des règles) ;
   - les barèmes 3 et 2 points (`M2_POINTS` dans `appliquerPack()`, `scoresM2()`, les appels
     `awardRow(…, 3)` et `awardRow(…, 2)` de `SCREENS.m2`, les textes « 3 points » et « 2 points ») ;
   - « 3 manches » sur l'écran titre.

   `validerPack()` exige en conséquence au moins 4 duos. Étendre l'auto-test à 4, 5 et 8 duos.
2. ~~Repère du prénom tapable~~ — fait le 24/09/2026 : `...` vaut `…`.
3. **Stockage hybride.** Fait le 24/09/2026, version minimale : ouvrir un `.json` (bouton ou
   glisser-déposer), l'enregistrer en téléchargement, et toujours afficher où en est
   l'enregistrement. Reste :
   - **Réécrire le même fichier** au lieu d'en télécharger une copie, via l'API File System Access
     (`showOpenFilePicker`, `createWritable`), qui n'existe que dans Chrome et Edge. Son
     fonctionnement depuis `file://` est **à vérifier**. Ailleurs, le téléchargement actuel sert de
     repli. Une page ouverte en double-clic ne peut pas non plus lire un JSON posé à côté d'elle :
     `fetch` est bloqué en `file://`.
   - **Version autonome** : embarquer le pack dans un `<script type="application/json" id="pack">`,
     lu au `BOOT` avant `PACK_DEFAUT`. Pour exporter, capturer
     `document.documentElement.outerHTML` au `BOOT`, **avant le premier `render()`** : après, le
     DOM contient l'écran dessiné. Et échapper `</` en `<\/` dans le JSON embarqué.
4. **Manches en modules.** Proposition, rien n'est codé : un registre `MODULES`, où chaque manche
   déclare ses règles, ses écrans, ses actions, ses touches, son onglet d'éditeur et sa validation.
   Le pack porte le déroulé :
   ```json
   "deroule": [
     { "module": "qualif", "sortants": 2 },
     { "module": "joker-categories", "tours": 3, "qualifies": 2, "points": { "joker": 3, "categorie": 2 } },
     { "module": "longueur-onde", "coeurs": 3 }
   ]
   ```
   Aujourd'hui, ajouter une manche oblige à toucher `SCREENS`, `act()`, le clavier, `regles()`,
   `roundMeta()`, `cleQuestion()`, `currentSponsor()` et `panelHTML()`. L'objectif : qu'un module
   neuf tienne dans un seul bloc. Garder les trois manches actuelles comme premiers modules, avec
   un comportement identique, vérifié par l'auto-test.

### Phase 3 — rendre l'éditeur accessible (pas commencée)

- **Vocabulaire** : remplacer « pack », « JSON », « Reset », « cible », « régime », « Éclair » et
  « Spotlight » par des mots de tous les jours, ou les expliquer sur place.
- **Chaque onglet** commence par ce que fait la manche, et un bouton « Voir à l'écran » prévisualise
  une question telle qu'elle sera projetée.
- **Erreurs** : une pastille rouge sur l'onglet concerné, plutôt qu'un bandeau de phrases. Le bouton
  « Jouer » dit pourquoi il est grisé.
- **Écran titre** : séparer « Préparer » de « Jouer ». L'éditeur n'affiche plus les boutons Reset et
  Scores du jeu.
- **Aide intégrée** remplaçant `docs/origine/`, obsolète : la finale y est décrite « au premier à
  3 points », et le contenu se modifie dans le code.

### Petits défauts connus

- « Reprendre en Manche 2 / 3 » efface la partie en cours sans confirmation.
- La classe CSS `.roles.qcm` n'est plus utilisée.
- L'accroche de l'écran de règles de la Manche 2 déborde sur deux lignes en 1366 × 768.
