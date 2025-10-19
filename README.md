# j2gl bytes

A personal blog built with Hugo and hosted on GitHub Pages. This site serves as a log of useful things I've done and how to do them again.

## Live Site

Visit the blog at: [https://j2gl.github.io/](https://j2gl.github.io/)

## Writing Posts

### Prerequisites

- [Hugo](https://gohugo.io/) (extended version)
- Git
- macOS with Homebrew (recommended)

### Setup

1. **Install Hugo**
   ```bash
   brew install hugo
   ```

2. **Clone the repository**
   ```bash
   git clone https://github.com/j2gl/j2gl.github.io.git
   cd j2gl.github.io
   ```

3. **Initialize the theme**
   ```bash
   git submodule update --init --recursive
   ```

4. **Run locally**
   ```bash
   hugo server -D
   ```

   Your site will be available at `http://localhost:1313`

### Useful Commands

- `hugo server` - Run without draft posts
- `hugo server -D` - Include draft posts
- `hugo server --open` - Auto-open browser
- `hugo new posts/my-post.md` - Create new post

## Writing Posts

### Create a New Post

```bash
hugo new posts/your-post-title.md
```

### Post Structure

```markdown
---
title: "Your Post Title"
date: 2025-10-19T10:00:00-07:00
draft: false
tags: ["tag1", "tag2"]
categories: ["category"]
description: "Brief description"
---

Your content here...
```

### Publishing

1. Set `draft: false` in your post's front matter
2. Commit and push to GitHub
3. GitHub Pages automatically builds and deploys

## Theme

This site uses the [PaperMod](https://github.com/adityatelange/hugo-PaperMod) theme, which provides:

- Clean, minimal design
- Dark/light mode toggle
- Fast search functionality
- SEO optimized
- Mobile responsive

## Project Structure

```
├── content/posts/          # Blog posts
├── themes/PaperMod/        # Theme (git submodule)
├── public/                 # Generated site (auto-built)
├── hugo.toml              # Site configuration
└── README.md              # This file
```

## Deployment

This site is automatically deployed to GitHub Pages when changes are pushed to the main branch. The deployment is handled by GitHub Actions.


## Contributing

It will be weird for someone to write in my log but feel free to open issues or submit pull requests with corresctions, bugs or have suggestions for improvements.

---

# Refernces
* [Hugo](https://gohugo.io/) 
* [PaperMod](https://github.com/adityatelange/hugo-PaperMod)