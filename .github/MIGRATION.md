# Homepage and blog publishing (2026-10-15)

The original generated Hexo files remain at the root of this repository's main branch. The workflow at .github/workflows/pages.yml composes the published website without changing those input files:

- /: homepage copied from lgswdn/home, main branch. The homepage does not link to the blog.
- /home/: still served by the existing home repository's GitHub Pages site.
- /blog/: existing generated Hexo website, with local URLs rewritten for the subdirectory.
- Existing article and archive URLs: generated HTML redirects preserve query strings and fragments. Old non-HTML asset/download URLs remain available too.

GitHub Pages Source must remain GitHub Actions. No blog repository is required for this arrangement.

## Updating the homepage

Edit the home repository as before. Its /home/ site updates using its existing deployment. To refresh the root site as well, open this repository's Actions, select Publish homepage and blog, then Run workflow on main. The workflow reads the latest home/main each time it runs. Changes in the home repository alone do not automatically trigger this repository's workflow.

## Before the next Hexo deployment from the other computer

hexo-deployer-git force pushes generated files, so deploying the unchanged local setup can remove the newly added workflow. First save this repository's .github/workflows/pages.yml to the local Hexo project at .github/workflows/pages.yml. Preserve any existing additional deployment directories, and add .github to deploy.extend_dirs. Set deploy.ignore_hidden to false so this directory is copied. Keep the current repository URL and main deployment branch.

Example fields to merge into the existing deploy configuration:

    deploy:
      type: git
      repo: <keep your existing repository URL>
      branch: main
      extend_dirs:
        - .github
      ignore_hidden: false

Set the Hexo website URL settings to:

    url: https://lgswdn.github.io/blog
    root: /blog/

The publication workflow accepts both the existing root-based generated links and already-prefixed /blog/ links. Check theme settings for hardcoded URLs. Verify the workflow is still present and that Actions succeeds after publishing.

Reference: https://github.com/hexojs/hexo-deployer-git#options

## Backup and rollback

The branch backup/blog-before-migration-20261015 preserves commit 541da37160a5899fa13f144e74ec6275fdead86e (the original generated blog). To restore the old root blog immediately, switch Settings > Pages > Source to Deploy from a branch, and select this backup branch and / (root). To restore the combined site, switch Source back to GitHub Actions and run Publish homepage and blog on main.
