# Agent guidance for this Drupal site

This codebase is a Composer-managed Drupal site. Local development uses `ddev`.

## Local environment (DDEV)

Run commands from the project root:

- Start or restart the local environment with `ddev start`, `ddev restart`, and `ddev stop`.
- Install PHP dependencies with `ddev composer install`.
- Open the site with `ddev launch`.
- Run Drush commands with `ddev drush <command>` such as `status`, `user:login`,  `cache:rebuild`, and `update:db`.

DDEV project config lives in `.ddev/config.yaml`. Use `.ddev/config.local.yaml` for machine-specific overrides.

## Common Drupal workflows

- Add a module with `ddev composer require drupal/<project>`, then  `ddev drush pm:enable --yes <module_machine_name>`, then `ddev drush cache:rebuild`.
- Apply database updates after code changes with `ddev drush update:db --yes`.
- Import repository configuration into the site with `ddev drush config:import --yes`.
- Export site configuration back to the repo with `ddev drush config:export --yes`.

## Guardrails

- Do not commit secrets or machine-local overrides such as `.env`, `settings.local.php`, or `.ddev/config.local.yaml`.
- Do not commit `vendor/` or uploaded files under `web/sites/*/files`.
- Do not edit Drupal core or contributed projects in place.
- Put custom code in `web/modules/custom` and `web/themes/custom`.

## Template-specific notes

### Content model

- Two content types: **Recipe** (`recipe`: summary, featured image, ingredients, preparation and cooking time, servings, difficulty, category and tags, directions in `field_content`) and **Article** (`article`: summary, featured image, tags, body in `field_content`). Both are translatable; the demo content ships in English and Spanish.
- Vocabularies: `recipe_category` and `tags`. Term pages list the tagged content.
- Pages (Home, Recipes, Articles, About Dashi, Page not found) are Drupal Canvas pages; recipe and article pages, cards and search results are Canvas content templates, so their layout is edited in Canvas, not in Twig.
- The heroes are chosen with flags: the front page shows the newest recipe that is promoted **and** sticky, the Recipes page the newest promoted recipe that is not sticky.
- Listings are views (`recipes`, `articles`, `promoted_items`, `featured_recipe`, `recipe_collections`, `content_terms`); the number of items in a list block can be changed in Canvas.

### Editorial workflow and roles

- Recipes and articles use the `basic_editorial` workflow from the Drupal CMS site template base (draft, published, unpublished).
- The `content_editor` role can create, edit, delete and translate recipes, articles, media, terms, pages and menu links, translate Canvas pages in the Canvas Translate workspace, and translate configuration (for example the page variant's footer texts). The demo ships eight editor accounts without passwords.

### Theme notes

- The theme is `dashi_theme`, generated from the Mercury starter kit and installed under `web/themes/contrib`. Colours, corner radius and shadows are CSS variables in `src/theme.css`, fonts in `src/fonts.css`; both can be overridden by copying them next to `index.php`, without rebuilding the CSS.
- Components are single-directory components under `components/`; CSS is built with `npm run build` in the theme directory.
- Search is indexed by cron; after a command-line install run `drush search-api:index` once.

### Deployment notes

- Cron must run for search indexing and scheduled publishing.

## References

- https://docs.ddev.com/en/stable/
- https://www.drupal.org/docs/administering-a-drupal-site/configuration-management/workflow-using-drush
