# Copycat 🐈‍⬛

Widget personnalisé [Grist](https://www.getgrist.com/) pour afficher un **tableau récapitulatif**, un **histogramme** et des **donuts** à partir d'une table, au style [DSFR](https://www.systeme-de-design.gouv.fr/).

Il sait **additionner** des colonnes (budgets, effectifs…) et **compter** des lignes (dossiers par statut…). Il remplace les deux anciennes versions du widget.

## Installation dans Grist

1. Ajouter un widget à la page : **Personnalisé**, puis **URL personnalisée**.
2. Coller l'URL : `https://jlllyfish.github.io/copycat/`
3. Choisir la table source et accorder l'accès **« Lire les données sources sélectionnées »**.
4. Associer les colonnes dans le panneau de droite (voir ci-dessous).

## Colonnes à associer

Toutes les colonnes sont facultatives. Le widget affiche ce qui est possible avec les colonnes associées.

| Champ du panneau | Type | Rôle |
|---|---|---|
| **Lignes du tableau** | tout type | Une ligne du tableau et un groupe de barres par valeur (ex. filière, département). |
| **Découper par (ex. statut)** | tout type | Découpe chaque ligne par valeur de cette colonne : une couleur par valeur. Active le comptage des lignes. |
| **Valeurs à additionner** | numérique (plusieurs possibles) | Colonnes additionnées par ligne (ex. budget demandé, budget alloué). |
| **Donut : répartition d'une colonne** | tout type | Part de chaque valeur de la colonne (ex. types d'avis). |
| **Donut : nombre** | numérique ou case à cocher | Part atteinte (une case cochée compte pour 1). |
| **Donut : total de référence** | numérique | Total à comparer. **Si vide, le total est le nombre de lignes.** |

### Exemples

- **Budgets par département** : Lignes = Département, Valeurs = Budget demandé + Budget alloué.
- **Dossiers par filière et par statut** : Lignes = Filière, Découper par = Statut, Valeurs = Montant.
- **% de dossiers complets** : Donut : nombre = case à cocher « Complet », total de référence laissé vide.

## Ce qui s'affiche

- **Tableau** avec une ligne de total. Avec « Découper par », c'est un tableau croisé : une colonne par valeur (ex. par statut).
- **Histogramme**. Quand plusieurs mesures existent (nombre de lignes, montants…), un sélecteur **Afficher** permet de passer de l'une à l'autre.
- **Donut « répartition »** et/ou **donut « part / total »**.
- **Filtres** sur « Lignes » et « Découper par ». Ils sont liés : chaque liste ne propose que les valeurs compatibles avec l'autre filtre, avec le nombre de lignes. Ils s'appliquent au tableau et aux graphiques. Leur état reste propre à chaque utilisateur et n'est pas enregistré dans le document.
- **Export PNG haute définition** de chaque graphique (copier ou télécharger), avec le titre et les filtres actifs.

## Configuration (bouton ⚙)

Les réglages sont enregistrés dans le document Grist, ce qui demande les droits d'édition. Ils sont rangés en sections repliables :

- **Textes** : titre, sous-titre, nom des éléments comptés (ex. « dossiers »), unité (ex. « € »), titres des colonnes et des graphiques.
- **Affichage** : éléments affichés, comptage des lignes, colonnes tracées dans l'histogramme, préréglage Démarches Simplifiées.
- **Ordre** : ordre d'apparition ou alphabétique, et valeurs à placer en premier (ex. `SP, EA, PP, MSC`).
- **Libellés et couleurs** de chaque colonne et de chaque valeur.

### Préréglage Démarches Simplifiées

Une case à cocher, activée par défaut, reconnaît les statuts DS : `en_construction`, `en_instruction`, `accepte`, `sans_suite`, `refuse`. Elle leur donne un libellé lisible, une couleur et un ordre fixes. La comparaison ignore la casse, les accents et les `_`.

## Accessibilité

- Le tableau sert d'alternative textuelle aux graphiques. Masqué à l'écran, il reste lu par les lecteurs d'écran.
- Chaque graphique a une description (`role="img"` + `aria-label`). Celle des donuts donne toutes leurs valeurs.
- Le panneau ⚙ est une fenêtre de dialogue accessible :
  - le focus reste à l'intérieur ;
  - Échap la ferme ;
  - le focus revient sur ⚙ à la fermeture.
- Les pastilles de couleur sont nommées (« Bleu France ») et se choisissent aux flèches du clavier.
- Le résultat des filtres est annoncé (`aria-live`).
- Couleurs :
  - celles de la palette ont un contraste d'au moins 3:1 sur fond blanc ;
  - le texte posé sur les donuts passe en noir ou en blanc selon le fond.
- Les animations sont désactivées quand l'utilisateur a choisi `prefers-reduced-motion`.

## Reprise des anciennes versions

Les noms techniques des colonnes sont ceux des anciennes versions, donc les colonnes déjà associées sont conservées.

| Ancienne version | Ce qui est conservé | À refaire |
|---|---|---|
| Version « somme » (Lignes / Valeurs / Donut) | tout | — |
| Version « dossiers » (Statut / Catégorie / Montant / Avis) | Statut, Catégorie, Avis, libellés, couleurs, symbole € | Réassocier **Montant** et **Montant 2** dans « Valeurs à additionner » |

## Développement

Un seul fichier, `index.html`, sans étape de compilation. Il charge depuis un CDN :

- [DSFR](https://github.com/GouvernementFR/dsfr) 1.13.1 (styles) ;
- [Chart.js](https://www.chartjs.org/) 4.4.4 et [chartjs-plugin-datalabels](https://chartjs-plugin-datalabels.netlify.app/) 2.2.0 ;
- [grist-plugin-api](https://support.getgrist.com/widget-custom/).

Pour tester hors de Grist, ouvrir la page affiche un message qui demande de l'ouvrir depuis un document Grist. Le plus simple est de pointer un widget de test vers la branche ou le fork publié sur GitHub Pages.

Dans le code :

- les colonnes sont définies dans `COLS` ;
- les réglages par défaut dans `DEFAULTS` ;
- la palette dans `PALETTE` ;
- le préréglage Démarches Simplifiées dans `DS_PRESET`.

Les clés `name` de `COLS` ne doivent pas changer : les associations faites dans les documents Grist en dépendent.
