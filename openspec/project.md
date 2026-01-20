# OpenSpec for OSS Latam Website

This document provides a comprehensive overview of the OSS Latam website project, its technical stack, architecture, and conventions, to be used as a reference for AI assistants and developers.

## Project Overview

*   **Project Name:** OSS Latam
*   **Description:** The official website for the Open Source Latin America (OSSLAT) community. It is a static website built with the Hugo framework, designed to be a central hub for news, blog posts, events, and information about the community.

## Tech Stack

*   **Static Site Generator:** [Hugo](https://gohugo.io/) (Extended version)
*   **Programming Language:** [Go](https://golang.org/) (as the foundation of Hugo)
*   **Package Management:** [npm](https://www.npmjs.com/) (for managing frontend dependencies and scripts)
*   **Content:** [Markdown](https://www.markdownguide.org/) is the primary format for all content.
*   **Templating:** Hugo Templates (Go-based)
*   **Styling:** [SCSS/SASS](https://sass-lang.com/) for stylesheets, compiled via Hugo's asset pipeline.
*   **Frontend JavaScript:** Vanilla JavaScript and libraries managed via npm.
*   **CI/CD:** [GitHub Actions](https://github.com/features/actions) for continuous integration and deployment.

## Architecture and Conventions

### Content Structure

*   The website is fully multilingual, supporting **English (en)**, **Spanish (es)**, and **Portuguese (pt)**.
*   All content is located in the `/content/` directory.
*   For every piece of content, there must be a corresponding file for each supported language, using the appropriate language code suffix. For example:
    *   `my-post.en.md` (English)
    *   `my-post.es.md` (Spanish)
    *   `my-post.pt.md` (Portuguese)
*   Content organization is directory-based. For example, all blog posts reside in `/content/blog/`, community news in `/content/news/`, and so on.

### Frontmatter

*   Every Markdown file must begin with a YAML frontmatter block.
*   The frontmatter is used to define metadata for the page, such as `title`, `date`, `authors`, `tags`, `categories`, and `images`.
*   The `title` should be translated for each language-specific file.
*   Other metadata like `date`, `authors`, and `tags` should remain consistent across all language versions of a post.

### Theme

*   The project uses a customized version of the `hugo-theme-bootstrap` located in the `/themes/` directory.
*   Layouts and partials are overridden in the root `/layouts/` directory to customize the theme's default behavior.

### Commits and Version Control

*   This project follows the **Conventional Commits** specification (e.g., `feat:`, `fix:`, `docs:`).
*   All changes are committed and pushed directly to the `main` branch. Pull requests are not the standard workflow for this project.

### CI/CD Workflow

*   A GitHub Actions workflow located at `.github/workflows/hugo.yaml` is responsible for building and deploying the Hugo site to GitHub Pages.
*   Another workflow, `.github/workflows/sync-openinfra.yml`, syncs specific content directories to an external repository. This action is triggered only by changes in the `content/openinfra/` path.