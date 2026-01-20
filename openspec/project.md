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

*   The website is fully multilingual, supporting **Spanish (es)** as the main language, followed by **English (en)** and **Portuguese (pt)**.
*   All content is located in the `/content/` directory.
*   For every piece of content, there must be a corresponding file for each supported language, using the appropriate language code suffix. For example:
    *   `my-post.es.md` (Spanish - Main)
    *   `my-post.en.md` (English)
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

### Submodules

*   The `/themes/` directory contains git submodules, such as `hugo-theme-bootstrap`.
*   **Do not** modify the contents of the `/themes/` directory directly within this project.
*   Any changes to the theme must be made in its own dedicated repository. After the changes are pushed to the theme's repository, the submodule in this project can be updated to point to the new commit.

### Commits and Version Control

*   This project follows the **Conventional Commits** specification (e.g., `feat:`, `fix:`, `docs:`).
*   All changes are committed and pushed directly to the `main` branch. Pull requests are not the standard workflow for this project.

### CI/CD Workflow

*   A GitHub Actions workflow located at `.github/workflows/hugo.yaml` is responsible for building and deploying the Hugo site to GitHub Pages.
*   Another workflow, `.github/workflows/sync-openinfra.yml`, syncs specific content directories to an external repository. This action is triggered only by changes in the `content/openinfra/` path.

## Image Specifications

To ensure a consistent and performant user experience, all images committed to this repository should adhere to the following guidelines.

### General Guidelines

*   **Optimization:** All images **must** be optimized before being committed. Use tools like [TinyPNG](https://tinypng.com/) / [TinyJPG](https://tinyjpg.com/), [ImageOptim](https://imageoptim.com/), or `squoosh` to reduce file size without significant quality loss. Aim for a file size under 150KB for banners and under 50KB for smaller images where possible.
*   **Format Selection:**
    *   **WebP (`.webp`):** Preferred format for all raster images (photographs, banners) due to its superior compression and quality characteristics.
    *   **PNG (`.png`):** Use for graphics that require transparency (e.g., some logos, icons).
    *   **JPEG (`.jpg`):** Use for photographs if WebP is not feasible. Avoid for graphics with sharp lines or text.
    *   **SVG (`.svg`):** Preferred for all logos, icons, and simple graphics. SVGs are resolution-independent and typically have a very small file size.
*   **File Naming:** Use descriptive, lowercase, kebab-case filenames (e.g., `sovereign-ai-stack-banner.webp`).

### Specific Image Types

#### Blog Post Banners

*   **Path:** `static/images/blog/<category>/` (e.g., `static/images/blog/openinfra/`)
*   **Aspect Ratio:** 1.91:1 (A common social media and card-friendly aspect ratio)
*   **Recommended Dimensions:** `1200px` by `630px`. This size works well for most social sharing platforms (e-g., Twitter, LinkedIn, Facebook).
*   **Format:** WebP

#### Author Images

*   **Path:** `static/images/authors/`
*   **Aspect Ratio:** 1:1 (Square)
*   **Recommended Dimensions:** `400px` by `400px`.
*   **Format:** WebP or JPEG.

#### Sponsor/Friend Logos

*   **Path:** `static/images/`
*   **Dimensions:** Maintain a consistent width for all logos in a list to ensure visual alignment. The current standard is `300px` width. The height should be auto-adjusted to maintain the logo's aspect ratio.
*   **Format:** SVG is highly preferred. If SVG is not available, use a transparent PNG.
