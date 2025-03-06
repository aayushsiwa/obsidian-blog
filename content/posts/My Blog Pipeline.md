---
title: MY BLOG PIPELINE
date: 2024-11-28
draft: false
tags:
  - blog
  - info
---

## **Obsidian**

Obsidian is an excellent note-taking app. Download it from [Obsidian.md](https://obsidian.md).

### **The Setup**

1. Create a new folder named `posts`.
2. Store all your posts inside this folder; the rest of the files will remain untouched.

---

## **Hugo**

### **Prerequisites**

- [Install Git](https://github.com/git-guides/install-git)
- [Install Go](https://go.dev/dl)

### **Install Hugo**

Follow the instructions at [Hugo Installation Guide](https://gohugo.io/installation).

### **Create a New Site**

```shell
# Verify Hugo installation
hugo version

# Create a new site
hugo new site <site-name>
cd <site-name>
```

### **Download a Hugo Theme**

1. Explore themes at [Hugo Themes](https://themes.gohugo.io).
2. Install a theme (e.g., PaperMod) as a Git submodule:
    
    ```shell
    git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
    ```
    
3. Follow the theme's documentation to complete the setup.

### **Sample `hugo.toml` File**

```toml
baseURL = "https://examplesite.com/"
title = "ExampleSite"
paginate = 5
theme = "PaperMod"
enableRobotsTXT = true

[params]
env = "production"
title = "ExampleSite"
description = "ExampleSite description"
keywords = ["Blog", "Portfolio", "PaperMod"]
author = "Me"
defaultTheme = "auto"
ShowReadingTime = true
ShowWordCount = true
```

### **Test Hugo Site**

```shell
hugo server -t PaperMod
```

> **Note**: The page may appear empty initially because no posts are added yet. Copy the `posts` folder from Obsidian into the `content` folder of your Hugo site.

---

## **Blog Properties**

Add properties (front matter) to your Markdown files. For example:

```yaml
---
title: My Blog Pipeline
date: 2024-11-28
draft: false
tags:
  - blog
---
```

> Use the Templater plugin in Obsidian to streamline this process.

---

## **Attachments**

To simplify image handling in Obsidian:

1. Set the default location for attachments to "In subfolder under current folder."
2. Add `../` before the image address to reference it correctly.

---

## **Deploy Hugo Site**

### **Build the Hugo Site**

```shell
hugo
```

> The output will be in the `public` folder. Deploy this folder to platforms like [Vercel](https://vercel.com) or [Cloudflare Pages](https://pages.cloudflare.com).

### **Automate Deployment**

Create a GitHub Action to automate building and deploying your site:

```yaml
name: Build and Deploy Hugo Site

on:
  push:
    branches:
      - master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4

    - name: Install Hugo
      run: |
        HUGO_VERSION="0.139.2"
        wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz
        tar -xvzf hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz
        sudo mv hugo /usr/local/bin/hugo
        rm hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz

    - name: Build Hugo Site
      run: |
        rm -rf public
        hugo

    - name: Deploy to GitHub
      env:
        GITHUB_TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      run: |
        git config user.name "GitHub Actions Bot"
        git config user.email "github-actions[bot]@users.noreply.github.com"
        git add .
        git commit -m "Deploy site $(date +'%Y-%m-%d %H:%M:%S')"
        git push --force "https://${{ secrets.PERSONAL_ACCESS_TOKEN }}@github.com/${{ github.repository }}" HEAD:master
```

### **Add `.gitignore`**

Prevent merge conflicts by ignoring the `public` folder:

```plaintext
# .gitignore
public
```

### **Setup Personal Access Token (PAT)**

1. Generate a PAT at [GitHub PAT Settings](https://github.com/settings/tokens).
2. Add it as a secret (`PERSONAL_ACCESS_TOKEN`) in your repository settings under **Settings > Secrets and Variables > Actions**.

---

With this setup, you can manage your Hugo blog seamlessly, integrating it with Obsidian for content creation and automating deployments via GitHub Actions.