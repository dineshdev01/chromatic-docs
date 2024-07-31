# Chromatic docs

The main branch is automatically deployed to https://www.chromatic.com/docs/

### Deployment

[![Netlify Status](https://api.netlify.com/api/v1/badges/3e1d4d54-1349-4c8a-b214-788ae7aac3a4/deploy-status)](https://app.netlify.com/sites/chromatic2-docs/deploys)

Available at docs.chromatic.com and via Netlify's build previews on branches/PRs. The 'website' proxies this to https://www.chromatic.com/docs/

To configure, access the Netlify [dashboard](https://app.netlify.com/sites/chromatic2-docs/overview).

Deploy previews are set up for PRs.

### 🚀 Project Structure

This project uses Astro. Inside, you'll see the following folders and files:

```
/
├── public/
│   └── favicon.svg
├── src/
│   ├── content/
│   ├── getStarted/
│   │   └── introduction.md
│   ├── notInNavigation/
│   ├── components/
│   │   └── Card.astro
│   ├── layouts/
│   │   └── Layout.astro
│   └── pages/
│       └── index.astro
└── package.json
```

### 🧞 Commands

All commands are run from the root of the project, from a terminal:

| Command                   | Action                                           |
| :------------------------ | :----------------------------------------------- |
| `pnpm install`            | Installs dependencies                            |
| `npm run dev`             | Starts local dev server at `localhost:4321`      |
| `npm run build`           | Build your production site to `./dist/`          |
| `npm run preview`         | Preview your build locally, before deploying     |
| `npm run astro ...`       | Run CLI commands like `astro add`, `astro check` |
| `npm run astro -- --help` | Get help using the Astro CLI                     |

## ✍️ Editing Content

Try to follow the conventions present.

⚠️ Note: Prettier [doesn't yet support MDX v2](https://arc.net/l/quote/iwcytzrp) so, for now, you'll need to format nested code snippets manually and wrap them with `prettier-ignore`. For example:

<!-- prettier-ignore-start -->

```html
{/* prettier-ignore-start */}

<IntegrationSnippets>
  <Fragment slot="storybook">
    ```bash
    # Use your project token and run the following command in your project directory
    npx chromatic --project-token <YOUR_PROJECT_TOKEN>
    ```
  </Fragment>
  <Fragment slot="playwright">
    ```bash
    # Run your Playwright tests as you normally would. For example:
    $ npx playwright test

    # Use your project token and run the following command in your project directory
    $ npx chromatic --playwright -t=<TOKEN>
    ```
  </Fragment>
  <Fragment slot="cypress">
    ```bash
    # Run your Cypress tests as you normally would along with the ELECTRON_EXTRA_LAUNCH_ARGS prefix
    $ ELECTRON_EXTRA_LAUNCH_ARGS=--remote-debugging-port=9222 yarn cypress run

    # Use your project token and run the following command in your project directory
    $ npx chromatic --cypress -t=<TOKEN>
    ```
  </Fragment>
</IntegrationSnippets>

{/* prettier-ignore-end */}
```

<!-- prettier-ignore-end -->

### Content

All content lives in `src/content`. Each file is a markdown file with frontmatter. The frontmatter is used to set the title, description, and other metadata. If you want to add a new page, create a new markdown file in the appropriate directory, and it will be automatically added to the site and linked in the sidebar. To prevent a page from being added to the sidebar, place it in `src/content/notInNavigation`.

### Links

Always use absolute links, that is, `/docs/<SOMETHING>`. Use `yarn check-links` to check for broken links. It runs against prod.

### Sidebar

The sidebar is autogenerated. Use the `sidebar` property in the frontmatter to control the order and label of the sidebar item. eg: `sidebar: { order: 1, label: 'Introduction' }`

### Media

Any static assets, like gifs and videos, can be placed in the `public/` directory. You'll have to manually add the `/docs` prefix for these urls.

Any static images added to `src` directory will be optimized and copied to `public/` during the build process.

Add the center css class to center media horizontally if they aren't full screen.

### Code

This project uses Shiki for syntax highlighting with the [transformerNotationDiff](https://shiki.style/packages/transformers#transformernotationdiff) and [transformerNotationHighlight](https://shiki.style/packages/transformers#transformernotationhighlight) transformers enabled.

Therefore, you can use Use `[!code ++]` and `[!code --]` to mark added and removed lines. For example:

```js
console.log("hewwo"); // [!code --]
console.log("hello"); // [!code ++]
console.log("goodbye");
```

And use `[!code highlight]` to highlight a line, like so:

```js
console.log("Not highlighted");
console.log("Highlighted"); // [!code highlight]
console.log("Not highlighted");
```

### Mermaid diagrams

You can insert Mermaid diagrams in markdown & MDX files to visualize Git commits and actions. For syntax and usage instructions, refer to the Mermaid documentation: [Mermaid documentation](https://mermaid.js.org/syntax/gitgraph.html).

### Adding an FAQ item

All faqs are in `src/content/faq`. Each FAQ item is a markdown file with frontmatter. The frontmatter is slightly different to other pages. You need to specify which `section` the item belongs to and optionally what order it appears in the section list (`sectionOrder`).

To add an FAQ item to an existing section, create a new `.md` or `.mdx` file. It'll automatically get added to the FAQ index page.

To add a new section, include the section in the `groupedFAQs` array in `src/pages/faq.astro` and add a new section to `src/content/troubleshooting/faq.mdx`

### Search

Algolia's Docsearch is integrated with the project. Every 24 hours it will crawl docs.chromatic.com and update it's index. The search input box is wired up to this index. You don't need to do anything special, whatever is pushed to docs.chromatic.com will be automatically indexed.

**Grouping and ranking of pages in search**

The sidebar order is used to rank pages in search results. And the search results are grouped by the sidebar group.

## Chromatic configuration options and schema

The Chromatic configuration options and schema are maintained in the `chromatic-config/options.json` file. This file is used to generate the Chromatic configuration options docs and the schema file.

Options are not included in the schema file by default. You have to add `"config.json"` to the `"supports"` array. eg: `"supports": ["CLI", "CI", "config.json"],`

To deprecate an option, set `deprecated` to `config.json` or `all`.
