# Organisation CA4

Application web pour **organiser, suivre et évaluer un cycle d'EPS en champ d'apprentissage 4** — « conduire et maîtriser un affrontement individuel ou collectif ».
Chaque élève reçoit un **niveau estimé** à partir de ce qui se passe réellement en match, et ce niveau sert à composer les équipes et à différencier le travail en ateliers.

Collège Louis Pasteur — Villemomble.

---

## Ce que fait l'appli

| Pendant le cours | Après le cours |
|---|---|
| **Appel** en un geste : vert présent, rouge absent, orange blessé | **Statistiques** de la classe, triables |
| **Équipes** composées automatiquement (équilibrées ou par niveau, mixtes, non mixtes ou sans contrainte) | **Fiche élève** : niveau, évolution, équipes, matchs |
| **Couleurs de chasubles** au choix, élève déplacé d'une équipe à l'autre en deux touches | **Évaluation** : grille par critères, note sur 20, export tableur |
| **Blessés** coachs d'une équipe ou dans « Autres rôles » (secrétaire, arbitre, chrono…) | **Banque de situations** pour les ateliers |
| **Planning des rencontres** sur plusieurs terrains | **Sauvegarde** et récupération des matchs d'une autre tablette |
| **Kiosque élève** sur tablette : score, entrées/sorties, buteurs, pertes de balle, tirs | |
| **Bilan de match** : rapport de force, graphique, stats simples | |

---

## Mettre l'appli en ligne sur GitHub Pages

1. Crée un dépôt sur GitHub (par exemple `organisation-ca4`), **public**.
2. Dépose **tout le contenu du dossier** à la racine du dépôt (bouton *Add file → Upload files*, puis glisse les fichiers et les dossiers `css`, `js`, `test`).
3. Dans le dépôt : *Settings → Pages → Build and deployment*, choisis *Deploy from a branch*, branche `main`, dossier `/ (root)`, puis *Save*.
4. Au bout d'une minute, l'adresse s'affiche : `https://ton-identifiant.github.io/organisation-ca4/`.
5. Ouvre cette adresse **une fois avec du réseau** sur chaque tablette : l'appli est ensuite disponible **hors ligne** au gymnase.
   Sur iPad : *Partager → Sur l'écran d'accueil* pour l'avoir comme une appli.

**Mettre à jour :** redépose simplement les fichiers modifiés sur GitHub. Au rechargement suivant (avec réseau), la nouvelle version s'affiche. Sans réseau, c'est la dernière version connue qui s'ouvre.

### Sans GitHub : le fichier autonome

`organisation-ca4-autonome.html` contient toute l'appli dans un seul fichier. Double-clique dessus, il s'ouvre dans le navigateur, sans réseau. Pratique sur une clé USB ou pour tester.

> ⚠️ Les données sont enregistrées **dans le navigateur qui a ouvert l'appli** (et pour le fichier autonome, à l'endroit où se trouve le fichier). Une tablette = ses propres données. Voir « Sauvegarde » plus bas.

Pour régénérer ce fichier après une modification : `node build.js` (Node.js suffit, aucune installation).

---

## Une leçon, pas à pas

1. **Accueil → Nouvelle classe** : colle ou choisis ton fichier CSV. Colonnes reconnues : `Nom`, `Prénom`, `Sexe` (F/G, fille/garçon, M…), ou une seule colonne `Élève` du type « DUPONT Marie ». Une colonne `Classe` avec plusieurs classes crée une classe par valeur. Une colonne `Niveau` (1 à 4) sert d'estimation de départ. Les exports Pronote (point-virgule, accents Windows) passent tels quels.
2. **Nouveau cycle** : choisis l'activité. La leçon 1 s'ouvre.
3. **Appel** : tout le monde est vert. Une touche → rouge (absent), deux → orange (blessé), trois → vert.
4. **Équipes → Composer les équipes** : nombre d'équipes, niveau, mixité. En « groupes de niveau » avec un nombre impair d'équipes, l'appli te demande s'il faut plus d'équipes fortes ou plus d'équipes faibles.
   - Touche un élève, puis l'équipe d'arrivée (ou un élève de cette équipe) pour le déplacer.
   - Touche le nom d'une équipe pour choisir la couleur des chasubles.
   - Un blessé placé dans une équipe devient **coach** : il reste avec ses camarades mais ne compte pas dans la moyenne.
5. **Règles** : ce que les élèves relèvent (qui est sur le terrain, qui a marqué, pertes de balle, tirs), le nombre de joueurs sur le terrain, la durée, et le **barème** (voir plus bas). Le bouton « Proposer selon le niveau de la classe » coche ce qui est raisonnable de la 6e à la terminale.
6. **Matchs** : « Faire le planning » répartit les rencontres sur tes terrains. « Mode kiosque » passe la tablette aux élèves.
7. **Leçon suivante** : « Nouvelle leçon » reprend les règles de la précédente, et dans Équipes, « Reprendre les équipes de la leçon N » recopie les équipes (sans les absents ; les blessés deviennent coachs).

---

## Le kiosque élève

- Plein écran, gros boutons, **aucun niveau, aucun plus/malus, aucune note** visibles.
- Les élèves choisissent leur match (ou en créent un), lancent le chrono, puis notent :
  - les **points** : un gros bouton par action du barème (les boutons se partagent la place selon leur nombre) ;
  - si « qui a marqué » est coché, les boutons de points sont **sur la ligne de chaque joueur** (plus une ligne « On ne sait pas qui ») ;
  - **Entrer / Sortir** : si l'équipe est au complet, « Entrer » demande directement qui sort (les joueurs concernés clignotent), et inversement ;
  - **Perte de balle** : le fond de l'écran prend la couleur de l'équipe qui a le ballon ;
  - **Annuler** : chaque équipe a son bouton, qui dit exactement ce qu'il retire (« Annuler : +100 Panier sans défenseur (Léa M.) »).
- En fin de match, les élèves voient un bilan simple : qui a dominé et quand, points, tirs réussis « 4 sur 10 », attaques, ballons perdus.
- **Sortir du kiosque** : cadenas en haut à droite, puis ton code (**1234** par défaut, à changer dans *Données → Réglages*).
- Code oublié ? Ajoute `?sortie-kiosque` à la fin de l'adresse (par exemple `…/organisation-ca4/?sortie-kiosque`) et recharge.

---

## Comment le niveau est estimé

**Le principe : le plus/malus sur le terrain.** À chaque point marqué, chaque joueur de l'équipe qui marque **et qui est sur le terrain** gagne ces points ; chaque joueur adverse sur le terrain les perd. Les remplaçants ne bougent pas. On connaît donc aussi le **temps de jeu** de chacun.

**Ramené au temps de jeu, puis comparé à la classe.** Le plus/malus est divisé par les minutes jouées (avec 4 minutes « à zéro » ajoutées, pour qu'un seul bon match ne propulse personne en tête), puis situé par rapport au reste de la classe sur l'échelle 1 à 4 :

| Niveau | Degré |
|---|---|
| 1 à 1,7 | Maîtrise insuffisante |
| 1,75 à 2,4 | Maîtrise fragile |
| 2,5 à 3,2 | Maîtrise satisfaisante |
| 3,25 à 4 | Très bonne maîtrise |

**Ton estimation de départ compte.** Au début, c'est ton estimation qui fait foi (2,5 si tu n'en donnes pas). Plus l'élève joue, plus le terrain prend le dessus : à 12 minutes de jeu, moitié-moitié. Tu peux aussi **fixer un niveau à la main** (onglet Niveaux du cycle, ou fiche élève) : il a toujours le dernier mot.

### Des barèmes différents d'une leçon à l'autre

Les élèves voient les points de la leçon (2, 10, 100…). Pour comparer les leçons entre elles, chaque action a aussi un **poids pour le niveau** :

- le plus petit point marqué « normal » vaut **1** ;
- les autres sont proportionnels jusqu'au double ;
- au-delà, le poids est **adouci** pour qu'une action très bonifiée compte plus sans écraser le reste du cycle.

Exemple : leçon 1, tous les paniers à 2 points → panier = 1. Leçon 2, panier à 10 et panier sans défenseur à 100 → 1 et 3,6 ; cerceau touché à 1 point → 0,1.
Tu peux écrire ta propre valeur dans l'onglet Règles (« auto » la remet au calcul).

Chaque match garde le barème du jour : changer le barème d'une leçon ne modifie jamais les matchs déjà joués.

### Tirs et possession

Dans le barème, indique pour chaque action si c'est **un tir réussi**, **un tir raté** ou **pas un tir** (un essai au rugby, un point au volley). Cocher « Tirs tentés » ajoute un bouton « Tir raté » à 0 point s'il n'y en a pas. Après un point marqué, le ballon passe à l'adversaire ; après un tir raté, il ne change pas (on ne sait pas qui prend le rebond).

---

## Sauvegarde et stockage

- Tout est enregistré dans le navigateur (clé `organisation-ca4:v1`, environ 5 Mo).
- Les photos et schémas sont réduits à 800 px avant d'être enregistrés. *Données → Stockage* montre la place utilisée et propose d'**alléger les images** si ça se remplit.
- *Données → Télécharger une sauvegarde* : un fichier `.json` à garder sur ton ordinateur ou ton ENT. **Fais-en une régulièrement.**
- *Données → Ouvrir une sauvegarde* : **Fusionner** (ajoute ce qui manque, garde le reste) ou **Tout remplacer**.
- **Deux appareils** (tablette des élèves + ton ordinateur) : dans la leçon, onglet Matchs, « Envoyer ces matchs » crée un petit fichier ; ouvre-le sur l'autre appareil avec *Données → Ouvrir*. Les matchs sont ajoutés sans rien écraser.

---

## Pour un collègue qui voudrait modifier le code

HTML, CSS et JavaScript classiques : pas de framework, pas de compilation, aucune dépendance. **Les fichiers déposés sont les fichiers exécutés.**

| Fichier | Objet global | Rôle |
|---|---|---|
| `js/core.js` | `APP` | état, enregistrement, CSV, petits outils |
| `js/ui.js` | `UI` | fenêtres, messages, couleurs de chasubles, graphiques SVG, images |
| `js/ratings.js` | `RATINGS` | estimation du niveau, poids du barème |
| `js/teams.js` | `TEAMS` | composition automatique des équipes |
| `js/arena.js` | `ARENA` | moteur de match (événements rejoués), bilan, planning |
| `js/progress.js` | `PROGRESS` | évolution leçon par leçon, historiques |
| `js/cloud.js` | `CLOUD` | sauvegardes, fusion, exports, stockage |
| `js/bank.js` | `BANK` | activités et banque de situations |
| `js/training.js` | `TRAINING` | ateliers différenciés |
| `js/eval.js` | `EVAL` | grille d'évaluation |
| `js/dossier.js` | `DOSSIER` | fiche élève |
| `js/lesson.js` | `LESSON` | écran de la leçon (appel, équipes, règles, matchs) |
| `js/kiosk.js` | `KIOSK` | mode kiosque et écran de saisie d'un match |
| `js/views.js` | `VIEWS` | accueil, classe, cycle, match, aiguillage des écrans |
| `js/main.js` | `MAIN` | démarrage, adresse, aiguillage des clics |

Conventions :

- **Clics** : `data-act="module.action"` ; **saisies** : `data-chg="module.action"`. Chaque module expose ses `acts` et `chgs`, `main.js` les rassemble. Un seul écouteur par type d'événement.
- `APP.set(fn)` enregistre et redessine ; `APP.setQuiet(fn)` enregistre sans redessiner (pendant la frappe).
- Fenêtres : `UI.modal({ title, body, okLabel, onOpen, onOk })` ; `onOk` qui renvoie `false` garde la fenêtre ouverte. Pas de `<dialog>.showModal()` (bloqué dans certains cadres et en `file://`).
- Un match n'enregistre que des **événements datés** ; score, temps de jeu, plus/malus et stats sont recalculés en les rejouant (`ARENA.replay`). Supprimer un événement remet tout d'aplomb.
- `sw.js` : réseau d'abord, cache si le réseau ne répond pas en 2,5 s. Il n'est activé que sur un vrai site (pas en `file://`).

### Tests

```
node test/check.js       # contraintes + équipes, matchs, niveaux, leçon
node test/csv.js         # import CSV
node build.js            # fabrique le fichier autonome
node test/browser.js     # parcours complet dans Chromium (fichiers séparés)
node test/browser.js --autonome
```

`test/browser.js` utilise Playwright s'il est installé (`npm install -g playwright && npx playwright install chromium`) ; sinon il s'ignore proprement. Playwright ne sert qu'aux tests, pas à l'appli.

Quand tu n'as encore aucune classe, *Accueil → Essayer avec une classe exemple* crée une classe fictive avec deux leçons et douze matchs simulés : pratique pour voir les statistiques sans attendre un cycle.
