# CodingCup Labs — Official Website

> The official, high-performance, neobrutalist website for CodingCup Labs. Built with Astro and Decap CMS.

## Table of Contents
1. [Tech Stack](#tech-stack)
2. [Architecture Overview](#architecture-overview)
3. [Project Structure](#project-structure)
4. [Design System](#design-system)
5. [Local Development](#local-development)
6. [Production Build](#production-build)
7. [Deployment](#deployment)
   - [GitHub Pages Setup](#github-pages-setup)
   - [GitHub Actions Pipeline](#github-actions-pipeline)
   - [Custom Domain](#custom-domain)
8. [Content Management (Admin CMS)](#content-management)
   - [Accessing the Admin Panel](#accessing-the-admin-panel)
   - [OAuth Setup (One-Time)](#oauth-setup)
   - [How to Write a Blog Post](#how-to-write-a-blog-post)
   - [How to Edit a Blog Post](#how-to-edit-a-blog-post)
   - [How to Delete a Blog Post](#how-to-delete-a-blog-post)
   - [How to Publish vs Save Draft](#how-to-publish-vs-save-draft)
9. [Adding Projects](#adding-projects)
10. [Environment Reference](#environment-reference)
11. [Troubleshooting](#troubleshooting)
12. [Contributing](#contributing)
13. [License](#license)

## Tech Stack
- **Framework:** Astro v4
- **Styling:** Vanilla CSS (CSS Custom Properties mapping to Stitch design tokens)
- **Content:** MDX via Astro Content Collections
- **CMS:** Decap CMS (Git-based, no database required)
- **Deployment:** GitHub Pages via GitHub Actions

## Architecture Overview
This is a purely static site. All pages are generated at build time. There is no server-side rendering (SSR) and no runtime database. Content is managed via Markdown files in the repository. Decap CMS provides an admin UI to edit these files directly in the browser, committing changes back to GitHub, which triggers a rebuild and deployment.

## Project Structure
```text
/
├── .github/workflows/deploy.yml   # GitHub Actions CI/CD pipeline
├── public/
│   ├── admin/config.yml           # Decap CMS configuration
│   ├── favicon.svg                # Site favicon
│   ├── og-image.png               # Open Graph image for social sharing
│   └── robots.txt                 # Search engine directives
├── src/
│   ├── layouts/                   # Astro layout components (Base, BlogPost)
│   ├── components/                # Reusable UI components (Navbar, Cards, etc.)
│   ├── pages/                     # Route definitions (index, about, etc.)
│   │   ├── admin/index.astro      # Decap CMS entry point
│   │   └── blog/                  # Blog listing and dynamic post routing
│   ├── content/                   # Content Collections
│   │   ├── config.ts              # Zod schema for blog frontmatter
│   │   └── blog/                  # Markdown files for blog posts
│   └── styles/
│       ├── tokens.css             # CSS variables extracted from Stitch Design
│       ├── global.css             # Base typography and resets
│       └── components.css         # Shared utility classes
├── astro.config.mjs               # Astro configuration
├── tsconfig.json                  # TypeScript configuration
└── package.json                   # Project dependencies
```

## Design System
The entire visual language of this site is derived from the **CodingCup Labs Neobrutalist Website** project in Stitch.
- **Single Source of Truth:** All colors, typography, spacing, and shadow values are mapped directly from Stitch into `src/styles/tokens.css`.
- **No Inventions:** We do not invent colors or arbitrary spacing values. If it's not in the design spec, it doesn't go in the CSS.
- **Key Characteristics:** High contrast (yellow/black), sharp corners (0px border-radius), hard offset shadows (4px/8px solid black), and thick structural borders (2px/4px).
- **Typography:** Space Grotesk (Headlines/Labels) and Inter (Body text), loaded via Google Fonts.

## Local Development

### Prerequisites
- Node.js >= 20.x (Recommend using nvm: `nvm install 20 && nvm use 20`)
- npm >= 9.x
- Git

### Setup
1. Clone the repository:
   ```bash
   git clone https://github.com/s-saanvi/CodingCupLabs.git
   cd CodingCupLabs
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev &
   ```
4. Open your browser and navigate to `http://localhost:4321`.

## Production Build
To create a production build locally:
```bash
npm run build
```
This will compile the static HTML, CSS, and optimized assets into the `/dist` directory. You can preview this build locally using:
```bash
npm run preview
```

## Deployment

### GitHub Pages Setup
This site is configured to deploy automatically to GitHub Pages using GitHub Actions.

1. Ensure the code is pushed to the `main` branch.
2. Go to your repository on GitHub.
3. Click **Settings** in the top navigation.
4. Click **Pages** in the left sidebar (under "Code and automation").
5. Under "Build and deployment", set the **Source** dropdown to **GitHub Actions**.
6. The deployment will trigger automatically on the next push, or you can trigger it manually via the **Actions** tab.
7. Once deployed, the site will be live at `https://s-saanvi.github.io/CodingCupLabs/` (or your configured custom domain).

### Custom Domain
If you want to use a custom domain:
1. In Settings → Pages → Custom domain, enter your domain name.
2. Add a `CNAME` file to the `/public` directory containing your domain name.
3. Configure your DNS provider (A records or CNAME).
4. Update `astro.config.mjs`:
   - Remove the `base: '/CodingCupLabs'` line.
   - Update the `site` value to your new custom domain (e.g., `site: 'https://codingcuplabs.com'`).

## Content Management (Admin CMS)
We use Decap CMS to manage blog content. It provides a visual editor that commits Markdown files directly to the GitHub repository.

### Accessing the Admin Panel
Navigate to `https://s-saanvi.github.io/CodingCupLabs/admin/` (or your custom domain equivalent).

### OAuth Setup (One-Time)
Decap CMS needs permission to commit to your GitHub repository. Since GitHub doesn't support implicit OAuth grants for SPAs, we need an OAuth proxy. We recommend using Netlify for this (it's free and doesn't require hosting the site there).

1. Create a free account at [Netlify](https://www.netlify.com/).
2. Go to User Settings → OAuth → GitHub → New OAuth App.
3. Set the Authorization callback URL to: `https://api.netlify.com/auth/done`
4. Netlify will provide a Client ID and Client Secret.
5. `public/admin/config.yml` is already configured to use Netlify as the proxy (`base_url: https://api.netlify.com`).

### How to Write a Blog Post
1. Log in to the `/admin/` panel using your GitHub account.
2. Click **Blog Posts** in the left sidebar.
3. Click the **New Blog Post** button in the top right.
4. Fill out the required fields (Title, Description, Published Date, Author).
5. Add Tags (press Enter after each tag).
6. Upload a Cover Image if desired.
7. Write your content in the Markdown body editor.
8. By default, posts are marked as "Draft" (`Draft: true`). Toggle this off when you are ready for the world to see it.
9. Click **Publish**. This commits the new file to the `main` branch.
10. GitHub Actions will automatically rebuild and deploy the site (takes ~1-2 minutes).

### How to Edit a Blog Post
1. In the `/admin/` panel, under **Blog Posts**, click on the post you want to edit.
2. Make your changes.
3. Click **Publish** to save and deploy.

### How to Delete a Blog Post
1. In the `/admin/` panel, under **Blog Posts**, click on the post you want to delete.
2. In the editor view, click the three-dot menu (⋮) in the top right corner.
3. Select **Delete**.
4. Confirm the deletion. This removes the file from the repository and triggers a rebuild.

## Adding Projects
The Projects portfolio is currently managed via a data array in `src/pages/projects.astro`. To add a new project:
1. Open `src/pages/projects.astro`.
2. Locate the `projects` array at the top of the file.
3. Add a new object following the existing structure:
   ```javascript
   {
     title: 'New Project Name',
     description: 'Description of the project.',
     tags: ['Tech1', 'Tech2'],
     link: 'https://project-url.com',
     category: 'Web'
   }
   ```

## Environment Reference
No `.env` file is required for local development or deployment since this is a fully static site. The only configuration needed is in `astro.config.mjs` (setting `site` and `base` for GitHub Pages).

## Troubleshooting

- **404 Not Found on GitHub Pages:** Ensure the `base` property in `astro.config.mjs` exactly matches your repository name (e.g., `/CodingCupLabs`).
- **Styles aren't applying:** Verify that `tokens.css`, `global.css`, and `components.css` are correctly imported in `src/layouts/BaseLayout.astro`.
- **Admin panel shows a blank page:** Ensure `public/admin/config.yml` exists and has the correct `repo` configured (`s-saanvi/CodingCupLabs`).
- **Images are broken:** When linking to assets in the `/public` folder from within `.astro` components, you MUST prefix the path with `import.meta.env.BASE_URL` (e.g., `src={import.meta.env.BASE_URL + "/image.png"}`).
- **Build fails in CI:** Check the Node version in GitHub Actions (must be 20+). Check the `npm ci` output for dependency conflicts.
- **Blog post not showing up:** Ensure `draft` is set to `false` in the frontmatter.

## Contributing
1. Create a feature branch (`git checkout -b feature/amazing-feature`).
2. Commit your changes (`git commit -m 'Add some amazing feature'`).
3. Push to the branch (`git push origin feature/amazing-feature`).
4. Open a Pull Request.
Please ensure all code follows the Neobrutalist design constraints outlined in the Design System section.

## License
MIT License — CodingCup Labs 2024
