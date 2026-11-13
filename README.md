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
