<h1 align="center">Spoke & Chain Craft Commerce Demo</h1>

![Spoke & Chain homepage](https://github.com/craftcms/spoke-and-chain/raw/stable/web/assets/volumes/images/Logos/logo.png)

## Overview

Spoke & Chain is a fictitious bicycle shop custom-built to demonstrate [Craft CMS](https://craftcms.com) and [Craft Commerce](https://craftcms.com/commerce). This repository houses the source code for our demo, which you can spin up for yourself by visiting [craftcms.com/demo](https://craftcms.com/demo?kind=spokeandchain).

We’ve also included instructions below for setting up the demo in a local development environment with [DDEV](https://ddev.com/).

Spoke & Chain shows core Craft CMS features and a fully-configured Craft Commerce store:

- Articles and pages with custom layouts and flexible content.
- Front-end global search for products and articles.
- Categorized products with variants, categories, filtering, and sorting.
- Customer membership area with subscription-based services, order tracking and returns, and account management.
- Full, customized checkout process with coupon codes.
- Configured for healthy SEO and built targeting WCAG AA compliance.

### Development Technologies

- [Craft CMS 4](https://craftcms.com/docs/4.x/)
- [Craft Commerce 4](https://craftcms.com/docs/commerce/4.x/)
- PostgreSQL (11.5+) / MySQL (5.7+)
- PHP (8.0.2+), built on the [Yii 2 framework](https://www.yiiframework.com/)
- Native Twig templates with reactive [Sprig](https://plugins.craftcms.com/sprig) components
- [Node](https://nodejs.org/en/) (12+) / [npm](https://www.npmjs.com/) (6+), managing and building front end resources

### Front End Dependencies

- [webpack](https://webpack.js.org)
- [Tailwind CSS](https://tailwindcss.com)
- [Alpine.js](https://alpinejs.dev)
- [Cypress](https://www.cypress.io)

## Local Development

### Environment

If you’d like to get Spoke & Chain running in a local environment, we recommend using [DDEV](https://ddev.com):

1. Clone the Spoke & Chain repository to your system.
    ```zsh
    git clone git@github.com:craftcms/spoke-and-chain.git spokeandchain && cd spokeandchain
    ```
1. Follow Craft related DDEV [installation instructions](https://craftcms.com/knowledge-base/migrating-from-craft-nitro-to-ddev).
1. Make sure you’ve used `ddev config` to create your environment.
1. Copy and update your `.env` file:
    ```zsh
    cp .env.example .env
    ```
1. Restore the initial database
   ```zsh
   ddev exec php craft db/restore seed.sql
   ```
1. Optionally seed demo data:
   ```zsh
   ddev exec php craft demos/seed
   ```
   > ⚠️ The Craft site is offline by default, and the seeder turns it on when it’s finished. If you skip this step, you’ll need to manually bring the site online by navigating to **Settings** → **General Settings** and switching **System Status** to “Online”.
1. Add a Craft account for yourself by following the prompts:
    ```zsh
    ddev exec php craft users/create --admin
    ```

> 💡 If you’re using a different local environment, see Craft’s [Server Requirements](https://craftcms.com/docs/3.x/requirements.html) and [Installation Instructions](https://craftcms.com/docs/4.x/installation.html).

### Front End

Run `npm ci` with node 12. (If you’ve installed [nvm](https://github.com/nvm-sh/nvm) run `nvm use`, then `npm ci`.)

If you’ve chosen a different environment setup, make sure your `.env` is configured for it. These environment variables are specifically used by `webpack-dev-server`:

- `DEVSERVER_PUBLIC`
- `DEVSERVER_PORT`
- `DEVSERVER_HOST`
- `TWIGPACK_MANIFEST_PATH`
- `TWIGPACK_PUBLIC_PATH`

You can then run any of the development scripts found in `package.json`:

- `npm run serve` to build and automatically run webpack with hot module reloading for local development
- `npm run build` to build front end assets for production

> 💡 When using `npm run serve`, switch your site’s URL from `https://` to `http://`.

#### PurgeCSS

This project uses PurgeCSS to automatically remove redundant or unused styles generated by Tailwind CSS.

PurgeCSS is disabled by default for the `serve` script, meaning your site will be loaded with every available CSS class. It also means you’ll need to check the site after running `build` to be sure important classes aren’t inadvertently stripped away.

Classes actively being used should be detected automatically, but you can encourage them to be recognized by making sure full class names appear in your template, stylesheet, and JavaScript files.

❌ For example, don’t dynamically combine `text-red-` with a variable for this loop:

```twig
{% set classes = ['100', '500', '900'] %}
{% for class in classes %}
  <div class="text-red-{{ class }}"></div>	
{% endfor %}
```

✅ Loop through complete class names like so they each appear in full:
```twig
{% set classes = ['text-red-100', 'text-red-500', 'text-red-900'] %}
{% for class in classes %}
  <div class="{{ class }}"></div>	
{% endfor %}
```

If you can’t avoid programmatic concatenation, use Tailwind’s [safelist](https://tailwindcss.com/docs/optimizing-for-production#safelisting-specific-classes) option in `tailwind.config.js`.

### Testing

Cypress tests cover multiple parts of the website:

- **control panel** – make sure the content structure is properly defined.
- **front end** – check that the website’s different sections work as expected.
- **accessibility** – evaluate the website for WCAG 2.0 compliance.

Set the environment variables Cypress needs to run by copying `cypress.example.json` to `cypress.json` and adjusting it:

```zsh
cp cypress.config.example.js cypress.config.js
```

Open the Cypress Test Runner from the project root:

```zsh
npx cypress open
```

Run accessibility tests only:

```zsh
npx cypress run --spec "cypress/e2e/front/a11y/*.cy.js"
```

## License

The source code of this project is licensed under the [BSD Zero Clause License](LICENSE.md) unless stated otherwise.

The imagery used by this project is the property of Marin Bikes, and used with permission. You are not free to use it for your own projects.
