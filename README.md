# CAWEB website checks

A small GitHub Actions workflow runs on every pushed commit, pull request, and manual run.

## What it checks

- Syntax of tracked PHP and plain JavaScript files (excluding vendor and node_modules).
- The current live French and English homepages on desktop and mobile.
- Successful page responses, expected language, content presence, and common WordPress error messages.
- Failed first-party assets, first-party JavaScript exceptions, and mobile menu opening/closing.
- Screenshots and results are saved in each run's website-report artifact for seven days.

## Current limitations

The active WordPress theme has not been added to this repository. Until its source and any custom plugins are imported, the syntax job explicitly warns that no website code was checked. Keep PHP files in their original theme/plugin directory structure. The runner currently uses PHP 8.3; match this to the university server once its version is confirmed. JSX, TypeScript, CSS quality, and plugin compatibility need additional checks if the imported source requires them.

The browser job tests https://mastercaweb.unistra.fr/ as currently deployed. It does **not** deploy or test changes in a pull request. A successful run is not proof that a proposed theme change works. It uses public pages only, without login or form submission. The tests use four Chromium sessions and do not crawl the whole site. They cover representative homepage behavior, not every page, form, browser, or accessibility requirement.

## Before using this as a release gate

1. Import the existing theme source and custom plugins from the previous team. Never commit wp-config.php, credentials, private database exports, or backups.
2. Obtain a staging WordPress instance matching production, with representative sanitized content and the same plugins.
3. Add an authorized deployment step to staging and run browser tests against that deployed commit before approving a release. The hardcoded SITE_URL in the workflow can then point to staging. Do not point it at an untrusted site.
4. Protect main: require pull requests, another person's review, and passing syntax and staging checks. Apply protections to administrators where supported. Repository visibility and plan determine feature availability.
5. Publish only the intended theme/plugin files after approval. Keep live content and uploads separate, clear the WordPress cache, and verify the site after deployment.
6. Arrange backups of files and database and a tested rollback procedure with university IT. Code rollback does not undo database changes.

No production deployment or server credentials are configured. If an existing live-site test fails, inspect its screenshot and fix or investigate the problem; do not weaken the assertion just to make the run green.

## Viewing results

Open Actions > Website checks > the latest run. Read both job results and the summary, and download website-report for screenshots and failure details. GitHub's normal Actions notification preferences control failure notifications.
