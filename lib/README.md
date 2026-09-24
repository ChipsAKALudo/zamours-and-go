# Librairies

Chargées par `zamours.html` avec `<script src="lib/…">`. Ce sont des versions « classiques » (UMD) :
les versions « modules » ne se chargent pas quand la page est ouverte en double-clic.

Le jeu fonctionne sans elles : sans `lib/`, il reste simplement muet et sans les animations GSAP.
**Pour transmettre le jeu, copier le dossier entier**, pas seulement le HTML.

| Fichier | Version | Source | Licence | SHA-256 |
|---|---|---|---|---|
| `gsap.min.js` | GSAP 3.15.0 | `npm pack gsap@3.15.0`, `dist/gsap.min.js` | [Standard « no charge »](https://gsap.com/standard-license) (gratuite, pas open source) | `92bb9a96476f983d212a2bc4f54c889039c1696dd4461d40a736860938570fbb` |
| `Tone.js` | Tone.js 15.1.22 | `npm pack tone@15.1.22`, `build/Tone.js` | MIT (`Tone.LICENSE.md`, `Tone.js.LICENSE.txt`) | `e290952fa43d9a7a780182a83c6fccf44d79cb7ae2cba102ef1f2b9d98124e22` |

Pour mettre à jour : refaire le `npm pack`, recopier le fichier, mettre à jour ce tableau, puis
relancer `zamours.html#autotest` et écouter une partie.
