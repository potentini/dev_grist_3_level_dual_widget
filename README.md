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
