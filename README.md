# dev_grist_3_level_dual_widget

Widget Grist dérivé du [widget Timeline](https://github.com/potentini/dev_grist_3_level_timeline_widget).

- La table reste en haut. Le bouton **Afficher la timeline** ou **Masquer la timeline** contrôle le panneau inférieur.
- Faites glisser la séparation pour régler la hauteur des panneaux. Les flèches haut et bas fonctionnent aussi quand la séparation a le focus.
- La timeline affiche par défaut le premier élément de niveau 1 et ses descendants. Sélectionnez une ligne pour afficher sa branche complète et mettre cette ligne en évidence.
- Cliquez sur une barre, un jalon ou une ligne de la timeline pour repérer l’élément dans la table. La branche affichée est conservée.
- Les modifications enregistrées dans l’un des panneaux rechargent les données communes depuis Grist.

## Installer dans Grist

1. Publiez ces fichiers à la racine de la branche `main` d’un dépôt. Dans **Settings → Pages**, choisissez **Deploy from a branch**, puis `main` et `/ (root)`.
2. Ajoutez un seul widget personnalisé dans Grist et indiquez l’URL GitHub Pages. Accordez l’accès complet au document pour le mapping multitable et l’édition.
3. Dans **Mapping**, choisissez les tables et colonnes sources. Associez les références vers les parents pour les niveaux 2 et 3 si utilisés. Les deux panneaux partagent cette configuration.
4. Cliquez sur une ligne de la table pour alimenter la timeline.

GitHub Pages renseigne automatiquement le SHA affiché à côté de **Mapping** lors du build Jekyll. La licence du widget d’origine est conservée dans `LICENSE`.

## Tri commun à la table et à la timeline

Deux sélecteurs règlent séparément les projets (titre par défaut) et leurs descendants (date de fin croissante par défaut). Les critères disponibles sont la fin, le début, le titre, la priorité mappée et l’ordre manuel. Les frères sont triés en conservant leur parent. Les dates manquantes sont placées en dernier. Sans date propre, un parent utilise la dernière fin ou le premier début de ses descendants. Les dates et les références ne sont pas modifiées par le tri.

Dans le mapping interne de chaque niveau, associer si besoin **Priorité (tri)** et **Ordre manuel**. La base de l’avancement est configurable : base 1 (`0,1` = 10 %), base 100 (`0,1` = 0,1 %), ou mode automatique historique. Les pourcentages décimaux sont conservés ; l’édition est convertie vers la base de la colonne source. Les colonnes calculées restent protégées.

Le code Table/Timeline est partagé avec `potentini/grist_project-like`. Pour les prochains reports, utiliser `scripts/export-dual-widget.py` depuis ce dépôt de suivi, puis vérifier les différences avant commit dans ce dépôt. Le script exporte `index.html`, `widget.js`, `hierarchy-sort.js` et `form-theme.css` ; les fonctions propres au suivi ne sont pas nécessaires au widget autonome. Les réglages Grist restent propres à chaque instance ; les modifications des données portent sur les tables sources.

La feuille partagée `form-theme.css` impose un texte foncé sur fond blanc dans les champs natifs et les listes de choix, en mode jour comme en mode nuit, y compris le texte de saisie automatique des navigateurs WebKit. Elle doit être publiée avec les fichiers du widget.

## Cadrage et navigation de la timeline

Le zoom **Tout** est proposé par défaut : il couvre la première et la dernière date du périmètre sélectionné, avec une marge de 5 % de cet intervalle de chaque côté (arrondie au jour supérieur, au minimum un jour). Les autres zooms couvrent respectivement **7 jours**, **4 semaines**, **3 mois (91 jours)** et **1 an (365 jours)**. Chaque période annoncée tient intégralement dans la largeur du panneau, même après redimensionnement.

Changer de zoom ou cliquer **Aujourd’hui** place la date du jour à environ 25 % de la période (arrondi au jour) : un quart vers le passé, trois quarts vers le futur. Cela reste vrai quand les données sont entièrement passées ou futures. Dans le zoom Tout, Aujourd’hui rétablit le cadrage des données avec ses marges. Les flèches et Aujourd’hui restent regroupés sur une seule ligne. Les anciens cadrages sont migrés vers Tout lors du premier chargement de cette version ; les choix suivants sont mémorisés.

## Commandes tactiles de la timeline

Avec **Édition autorisée**, un appui court sur une barre ouvre la fenêtre de modification des données. Un appui long (environ une demi-seconde) active cette seule barre : elle est entourée de bleu et ses poignées de début/fin apparaissent. Glisser ensuite le centre déplace les dates en conservant la durée ; glisser une poignée ajuste une seule date. Les jalons se déplacent aussi. Un nouvel appui long sur la barre active, ou un appui court/long sur un autre élément ou en dehors de la timeline, désactive cette sélection. Les mêmes gestes fonctionnent dans la timeline générale, la timeline projet et le dual widget original. La souris conserve ses commandes habituelles.

Les boutons **↶ / ↷** annulent ou rétablissent les mises à jour de dates et de champs faites dans cette vue Table/Timeline. Cet historique local (100 modifications maximum) est remis à zéro au rechargement, à la fermeture de la timeline projet, ou après une création/suppression. Il ne couvre pas les créations/suppressions ni les modifications effectuées dans le panneau de suivi ou une autre vue Grist. Une nouvelle modification efface les possibilités de rétablissement. Si les champs concernés ont changé entre-temps, l’annulation est refusée pour préserver ces changements.

Annuler le geste (ou Échap) rétablit l’affichage sans enregistrer. Les dates calculées, les périodes agrégées sans dates propres et les éléments sans colonnes sources éditables restent protégés. En lecture seule, les barres permettent le défilement tactile et aucune date n’est modifiée. Les confirmations n’interceptent pas les appuis.

Les boutons natifs et dialogues utilisent également des couleurs explicites, indépendantes du thème système. Les commandes colorées conservent un texte contrasté ; les libellés intérieurs de timeline choisissent noir ou blanc selon la couleur de la barre, avec un fond stable sous le texte pendant l’affichage de l’avancement. `tests/control-contrast.cjs` dans le dépôt de suivi vérifie les boutons et textes sur fonds propres, les dialogues et l’éditeur de timeline en mode clair et sombre.
