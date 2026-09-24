# Z'amours and Go

Jeu de soirée entre collègues, façon « Mr & Mrs » : des duos doivent deviner les réponses l'un de l'autre.
Un seul écran, projeté, piloté au clavier par l'animateur.

## Lancer une partie

Double-cliquer sur `zamours.html` (Chrome ou Edge conseillés), puis `F11` pour le plein écran.
Aucune connexion nécessaire.

Tout se pilote au clavier : `→` avancer (et dévoiler les classements place par place), `←` revenir,
`Espace` chrono, `R` révéler l'option suivante d'un QCM (`Maj+R` en retire une), `1` à `9` choisir le
thème en Manche 2, `S` scores, `M` couper ou remettre le son. Pendant le jeu, les boutons se cachent
pour que la salle ne voie que le jeu : bouger la souris ou appuyer sur `Échap` les fait réapparaître.

Le son démarre au premier appui sur une touche (les navigateurs l'exigent). **Pour transmettre le
jeu, copier le dossier entier** : sans le dossier `lib/`, il marche encore, mais muet et moins animé.

## Vérifier après une modification

Ouvrir `zamours.html#autotest` : la page joue une partie complète avec le pack chargé, sans rien
sauvegarder, et affiche en bas à gauche ce qui casse. À relancer après toute modification du moteur.

## Faire évoluer le jeu avec Claude

Ouvrir Claude dans ce dossier : il lit `CLAUDE.md` (architecture, pièges, décisions prises, reste à
faire) avant de toucher au code.

## Garder sa partie

Le navigateur garde la partie en cours de préparation, mais vider ses données ou changer de PC la
fait perdre. **Clique sur « 💾 Enregistrer » à la fin de chaque séance de préparation** : la partie
part dans tes Téléchargements, en fichier `.json`. Pour la reprendre, « 📂 Ouvrir une partie », ou
glisse le fichier sur la page. L'éditeur indique toujours si tes dernières modifications sont
enregistrées.

## Dossiers

- `docs/origine/` — fiche animateur et manuel de la première édition (juillet 2026). En partie
  obsolètes : la finale se joue désormais aux cœurs, et le contenu se modifie dans l'éditeur, plus
  dans le code.

## Feuille de route

1. Remise d'aplomb du moteur — corrections et auto-test.
2. Manches en modules, déroulé et barèmes dans le pack, pack enregistré dans un fichier JSON.
3. Éditeur lisible par quelqu'un qui n'est pas tech, aide intégrée, `CLAUDE.md` pour les évolutions.
