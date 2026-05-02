# Content Management (Admin CMS)
We use Decap CMS to manage blog content. It provides a visual editor that commits Markdown files directly to the GitHub repository.

## Accessing the Admin Panel
Navigate to `https://s-saanvi.github.io/CodingCupLabs/admin/` (or your custom domain equivalent).

## OAuth Setup (One-Time)
Decap CMS needs permission to commit to your GitHub repository. Since GitHub doesn't support implicit OAuth grants for SPAs, we need an OAuth proxy. We recommend using Netlify for this (it's free and doesn't require hosting the site there).

1. Create a free account at [Netlify](https://www.netlify.com/).
2. Go to User Settings → OAuth → GitHub → New OAuth App.
3. Set the Authorization callback URL to: `https://api.netlify.com/auth/done`
4. Netlify will provide a Client ID and Client Secret.
5. `public/admin/config.yml` is already configured to use Netlify as the proxy (`base_url: https://api.netlify.com`).

## How to Write a Blog Post
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

## How to Edit a Blog Post
1. In the `/admin/` panel, under **Blog Posts**, click on the post you want to edit.
2. Make your changes.
3. Click **Publish** to save and deploy.

## How to Delete a Blog Post
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
