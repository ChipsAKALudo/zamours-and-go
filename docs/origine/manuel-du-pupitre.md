---
type: documentation
tags: [jeu, boulot, animation, happiness, evalandgo, doc]
created: 2026-07-27
dernier_travail: 2026-07-27
lien: "[[🌹 Z'amours and Go — Fiche animateur]]"
---

# Manuel du pupitre — Z'amours and Go

Mode d'emploi de l'interface de projection `zamours-console.html`.
La **fiche animateur** contient les règles et les questions ; ce manuel ne parle que de l'outil.

**Fichier local :** `20-Idées/Projets/zamours-console.html`

> ⚠️ **Un seul écran, projeté.** Tout ce qui s'affiche est vu par la salle. L'outil ne contient plus aucune consigne qui te soit adressée — celles-ci vivent uniquement dans la fiche animateur. Si tu ajoutes du texte au fichier, garde ce principe en tête.

---

## 1. Avant la soirée — 5 minutes de préparation

1. **Ouvre la page** (double-clic sur le fichier local — il marche sans connexion).
2. **Branche le projecteur** et passe l'affichage en **mode Dupliquer** (touche `Win` + `P` sur Windows). Tout le monde voit le même écran, toi compris : il n'y a pas de vue cachée à gérer.
3. **Plein écran** : `F11`. La page est conçue pour tenir dans un écran sans jamais scroller.
4. **Fais un tour à blanc** : lance la Manche 1, avance de deux questions, attribue un point, ouvre les scores. Puis clique **⟲ Reset** en haut à droite.

> ⚠️ **Fais ce tour à blanc sur la machine et le projecteur du jour J**, pas sur ton PC à la maison. C'est le seul moyen de voir si le texte est lisible depuis le fond de la salle.

**À prévoir à côté de l'écran :** 6 ardoises + feutres + chiffons · les 2 buzzers poulets · le petit prix du Fail.

---

## 2. Les cinq touches

Tout se pilote au clavier. Tu n'auras pas à chercher la souris devant la salle.

| Touche | Effet |
|:--|:--|
| <kbd>→</kbd> | Avancer — question suivante, duo suivant, manche suivante |
| <kbd>←</kbd> | Revenir en arrière |
| <kbd>Espace</kbd> | Lancer / arrêter le chrono de 20 secondes |
| <kbd>R</kbd> | Révéler **l'option suivante** d'un QCM (une par une) |
| <kbd>S</kbd> | Ouvrir / fermer le panneau des scores |

Chaque bouton à l'écran affiche son raccourci. Si tu oublies, c'est écrit dessus.

**Attribuer les points se fait à la souris** : tu cliques le nom du duo qui a matché. C'est le seul geste souris de la soirée, et il est volontairement gros et lent — c'est le moment où tu regardes la salle, pas l'écran.

**⟲ Reset** est disponible en haut à droite de **tous** les écrans. Il demande confirmation, efface les scores et ramène au titre. Il est là pour enchaîner les tests à blanc ; en soirée, ne le touche pas.

---

## 3. Le déroulé, écran par écran

### Les animations

Deux animations rythment la partie, tu n'as rien à déclencher :

- **Le splash de manche** — à l'ouverture de chaque manche : le numéro en géant, le titre, et ce qui est en jeu. ~2 s.
- **L'annonce de duo** — en Manche 2, à chaque changement de duo ou d'étape : « Maintenant, c'est à… » avec le nom du duo, puis la question qui monte en fondu. ~1,8 s.

Elles ne se rejouent pas quand tu attribues un point, révèles une option ou ouvres les scores. Elles se rejouent si tu reviens en arrière avec <kbd>←</kbd>, ce qui aide à se resituer.

> Les durées sont réglables en haut du bloc script : `SPLASH_MS` et `ANNOUNCE_MS`. Si ça traîne au test à blanc, descends l'annonce à 1300.

### Écran de titre

Rappelle la règle de la remise à zéro. **Dis-la aussi à voix haute avant de lancer la Manche 1.** Si tu l'annonces après coup, les éliminés auront l'impression d'avoir joué pour rien.

<kbd>→</kbd> pour commencer.

### Manche 1 — les 11 questions

En haut : le numéro de question et une barre de progression.
En dessous : une étiquette **⚡ Éclair** ou **🔦 Spotlight** qui rappelle le régime de révélation, et le nombre de points.

Le déroulé d'une question :

1. Tu lis la question à voix haute.
2. <kbd>Espace</kbd> — le chrono part. L'anneau se vide, les 5 dernières secondes passent en rose.
3. Les cibles écrivent. À zéro, feutres en l'air.
4. **Éclair** → tout le monde retourne son ardoise en même temps, tu scannes.
   **Spotlight** → tu interroges duo par duo, et tu retournes chaque ardoise après la réponse.
5. Tu cliques le nom de chaque duo qui a matché. La pastille passe au vert.
6. <kbd>→</kbd> pour la suivante.

**La cible est affichée dans la pastille** : prénom souligné en ambre = c'est lui/elle qui écrit la vraie réponse. Elle est tirée au sort au démarrage et **alterne toute seule à chaque question**. Rien à gérer. (Voir §4 pour corriger un départ.)

**Tu t'es trompé de duo ?** Re-clique dessus : le point est retiré. Rien n'est définitif.
**Tu veux revoir la question précédente ?** <kbd>←</kbd>. Les points déjà attribués sont conservés.
**Le chrono est parti trop tôt ?** Bouton *Remettre à 20*.

### Écran d'élimination

Le classement s'affiche automatiquement, du meilleur au moins bon.

- **Clique les 2 duos qui sortent.** Ils se marquent en rose avec une croix. Re-clique pour annuler.
- Le bouton *Manche 2* reste **désactivé tant que tu n'en as pas exactement 2**. C'est volontaire : impossible de partir en Manche 2 avec un entonnoir bancal.
- **Si des duos sont à égalité sur la barre**, l'écran affiche un bandeau ambre et marque les duos concernés d'un trait ambre à gauche. Tu fais la mort subite, puis tu cliques.

C'est ici que tu **remets les buzzers poulets** aux deux duos sortants.

### Manche 2 — trois tours de table

**⚠️ Le déroulé a changé.** On ne fait plus les 3 questions d'un duo à la suite : on tourne **par étape**, tous les duos à chaque tour.

1. **Tour 1** — le Joker de chaque duo, l'un après l'autre.
2. **Tour 2** — chaque duo annonce sa catégorie à son tour, 1 question tirée.
3. **Tour 3** — idem, nouveau choix possible.

L'en-tête indique en permanence quel duo joue, à quelle étape, et son rang dans le tour (`Duo 2/4`).

- **Le Joker** s'affiche directement. Les deux faux départs (Sophie & Flo, Aurélie & Nico) apparaissent **barrés au-dessus de la vraie question** : lis-les tels quels, le gag est fait pour être projeté.
- **La Catégorie** : un écran propose les 6 thèmes, avec **le nombre de questions restantes entre parenthèses**. Le duo annonce le sien, tu cliques, l'app tire **1 question au hasard** dedans.
- **Pioche commune** — la question tirée est retirée de la banque pour toute la manche. Deux duos sur le même thème ne peuvent pas tomber sur la même question. Un thème épuisé se désactive tout seul (impossible avec la banque actuelle : 50 questions pour 8 tirages).
- **Attribuer** : un seul bouton *Accordé*, puisqu'un seul duo joue à la fois.

> Le bouton *Suivant* est désactivé tant que le duo n'a pas annoncé sa catégorie.

### Les QCM — le point à ne pas rater

Dans la catégorie **🎭 Scénarios**, sur une question de **🏠 Vie perso** et sur deux Jokers, les options sont **masquées par défaut**.

**C'est l'effet le plus fragile de la soirée :** si les options apparaissent avant que tu aies lu l'intitulé, la blague tombe à plat.

1. Lis **l'intitulé seul**, à voix haute. Laisse un temps.
2. <kbd>R</kbd> — **la première option apparaît**. <kbd>R</kbd> à nouveau pour la deuxième, puis la troisième. Tu peux les commenter au fur et à mesure.
3. Un <kbd>R</kbd> de plus après la dernière **remasque tout** et repart de zéro, si tu as révélé trop vite.
4. Rappelle la règle : **les deux partenaires doivent choisir la même lettre**.

> Sur un QCM, il n'y a pas de cible : le pupitre retire automatiquement le surlignage du prénom et affiche « les DEUX doivent avoir choisi la même lettre » à la place du nom de l'arbitre.

### Écran des finalistes

Même logique que l'élimination, mais **la pastille marque « qualifié », pas « éliminé »** — l'écran te le rappelle sous le classement, parce que le geste est identique et que la confusion est facile en direct.

Deux sorties possibles :

- **Lancer la finale** — actif dès que 2 duos sont désignés.
- **Clore la partie ici** — bouton discret à gauche. Le vainqueur est alors désigné sur le score de la Manche 2, et l'écran de clôture s'adapte tout seul. C'est le chemin prévu si le temps manque ou si la salle décroche : **tu n'es pas obligé de jouer la finale.**

### Manche 3 — la finale

Les deux scores des finalistes sont affichés en permanence en haut à droite. Pas de cible ici : les deux membres écrivent à l'aveugle.

Chaque clic sur un duo lui **ajoute 1 point** (ici le clic n'est pas réversible — c'est un compteur, pas une bascule). <kbd>→</kbd> passe à la question suivante ; les 7 questions tournent en boucle si tu les épuises.

Quand quelqu'un atteint 3, clique **Déclarer le vainqueur**.

### Clôture

Le vainqueur en grand, le score, et un rappel : **le Jury du Fail a le mot de la fin**. Ne coupe pas là — c'est leur moment.

Si les deux finalistes sont à égalité, l'écran affiche « Mort subite » au lieu d'un vainqueur.

---

## 4. Le panneau des scores

<kbd>S</kbd> l'ouvre par la droite, par-dessus le jeu. Il contient :

- **Le tableau Manche 1** — classement, avec les éliminés grisés et barrés.
- **Le tableau Manche 2** — séparé, puisqu'il repart de zéro. Les deux ne s'additionnent jamais, ni à l'écran ni dans les calculs.
- **La finale**, si elle a commencé.
- **Le Jury du Fail** et son rôle.

**Le sélecteur de départ.** Chaque ligne porte un bouton `départ : Prénom`. La règle de la cible est désormais tranchée : **tirage au sort au démarrage, puis alternance automatique à chaque question**, indépendamment pour chaque duo. Ce bouton ne fige pas la cible — il **inverse le point de départ** de l'alternance pour ce duo. Utile si un duo a une contrainte (quelqu'un qui doit passer en premier, une question qui tombe mal).

---

## 5. Si ça tourne mal

| Situation | Quoi faire |
|:--|:--|
| **L'onglet s'est fermé / le PC s'est mis en veille** | Rouvre la page. **Tout est sauvegardé automatiquement** — scores, éliminations, position dans le jeu, cibles. Tu reprends exactement où tu étais. |
| **Tu as attribué un point au mauvais duo** | Re-clique dessus (Manches 1 et 2). En finale, le compteur ne se décrémente pas : passe par *⟲ Reset* seulement en dernier recours. |
| **Tu as éliminé le mauvais duo** | Re-clique pour le désélectionner, tant que tu n'as pas quitté l'écran. |
| **Tu es déjà en Manche 2 et l'élimination était fausse** | Il n'y a pas de retour arrière entre les manches. Continue, et corrige à l'oral. |
| **Tu as révélé une option QCM trop tôt** | <kbd>R</kbd> jusqu'à repasser par le masquage complet, puis reprends. |
| **Le texte est trop petit au fond de la salle** | `Ctrl` + `+` dans le navigateur. La mise en page suit. |
| **Le chrono ne repart pas** | *Remettre à 20*, puis <kbd>Espace</kbd>. |
| **Une animation reste bloquée à l'écran** | *⟲ Reset* les nettoie. Sinon, `F5` : la partie est sauvegardée. |
| **Tu veux tout effacer** | *⟲ Reset*, en haut à droite de n'importe quel écran. Une confirmation est demandée. |

---

## 6. Ce que le pupitre ne fait pas

À savoir pour ne pas le chercher en direct :

- **Il ne connaît aucune réponse.** Il n'y a pas de questionnaire pré-rempli : la vérité est sur l'ardoise de la cible, et c'est **elle** qui valide si le devineur a matché. L'outil ne fait qu'afficher et compter.
- **Il ne fait pas de bruit.** Pas de son de fin de chrono. Les seuls effets sonores de la soirée sont les poulets.
- **Il ne gère pas la mort subite.** Quand il détecte une égalité, il te prévient, mais c'est toi qui poses la question et qui cliques le résultat.
- **Il n'affiche pas de vue séparée pour la salle.** Un seul écran, tout le monde voit la même chose.
- **Il ne revient pas en arrière entre les manches.** À l'intérieur d'une manche, <kbd>←</kbd> fonctionne.

---

## 7. Modifier les questions

Tout le contenu est en haut du fichier `zamours-console.html`, dans un bloc lisible qui commence par `/* ===== CONTENU ===== */`.

- `DUOS` — les paires, dans l'ordre. Retirer un duo ou en ajouter un se fait ici, tout le reste s'adapte.
- `M1` — les questions de la Manche 1, avec leur type (`eclair` / `spotlight`) et leurs points.
- `JOKERS` — un par duo, indexé sur son identifiant. `gag` affiche un faux départ barré au-dessus ; `opts` transforme la question en QCM.
- `CATEGORIES` — les 6 thèmes et leurs questions. `opts` associe un QCM à une question précise, **par son index dans le tableau `qs`** (0 = première question).
- `M3` — les questions de la finale.

**⚠️ Attention aux index `opts`.** Si tu insères une question au milieu d'un tableau `qs`, tous les index suivants se décalent et les options du QCM se retrouvent sur la mauvaise question. **Ajoute toujours en fin de liste**, ou renumérote les `opts` à la main.

Après modification, recharge la page et **clique ⟲ Reset** : une partie sauvegardée avec l'ancienne liste peut afficher n'importe quoi.

> **Si tu changes la structure de l'état** (pas juste du texte), incrémente la constante `KEY` en haut du bloc `ÉTAT` — actuellement `"zamours-console-v2"`. Ça invalide proprement les parties sauvegardées à l'ancien format et évite de débugger en direct. C'est exactement ce qui a causé l'unique plantage rencontré en développement.
