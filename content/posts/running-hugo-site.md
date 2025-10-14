---
title: "Running This Hugo Blog Locally"
date: 2025-10-14T09:30:00+02:00
draft: false
tags: ["hugo", "development"]
categories: ["setup"]
description: "Quick guide to run this Hugo blog on your local machine"
---

## Inception: Running Hugo Locally

Want to preview changes or write posts offline? Here's how to get this Hugo site running on your local machine.

### Prerequisites

Make sure you have these installed:
- Git
- Homebrew (for MacOS)

### Step 1: Install Hugo

```sh
brew install hugo
```

Verify installation:
```sh
hugo version
```

### Step 2: Clone and Setup

If you haven't already, clone the repository:
```sh
git clone git@github.com:j2gl/j2gl.github.io.git
cd j2gl.github.io 
```

Initialize the theme submodule:
```sh
git submodule update --init --recursive
```

### Step 3: Run Locally

Start the development server:
```sh
hugo server -D
```

Your site is now running at `http://localhost:1313`

### Useful Commands

- `hugo server` - Run without draft posts
- `hugo server -D` - Include draft posts
- `hugo server --open` - Auto-open browser
- `hugo new posts/my-post.md` - Create new post

### Making Changes

- Edit files and see live updates
- Press `Ctrl+C` to stop the server
- Changes to config require restart

### Publishing

When ready to publish:
1. Set `draft: false` in your post
2. Commit and push to GitHub
3. GitHub Pages automatically deploys

That's it! Now you can write and preview posts locally before publishing.