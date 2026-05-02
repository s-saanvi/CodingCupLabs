# Local Development & Deployment Setup

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
