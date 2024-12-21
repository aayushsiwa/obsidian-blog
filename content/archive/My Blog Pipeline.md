---
title: My First Blog
date: 2024-11-28
draft: false
tags:
  - blog
  - info
---


## Obsidian
- Obsidian is a good notes app, download it form https://obsidian.md
### The Setup
	Create a new folder labelled "posts".
	We will only be posting stuff inside this folder and rest of the files will remain untouched.

Now, let's setup Hugo

## Hugo

### Prerequisites
- Install Git https://github.com/git-guides/install-git
- Install Go https://go.dev/dl

### Install Hugo
https://gohugo.io/installation

### Create a new site
```shell
## check hugo installation
hugo version

## create a new site
hugo new site <site-name>
cd site-name
```

### Download Hugo theme
- Find themes at https://themes.gohugo.io
- best way to install a theme is *as a  git submodule*
	`git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod`
	
	follow instructions to setup theme and configuration file

### Sample hugo.toml file
```toml
baseURL = "https://examplesite.com/"
title = "ExampleSite"
paginate = 5
theme = "PaperMod"
enableRobotsTXT = true
buildDrafts = false
buildFuture = false
buildExpired = false
pygmentsUseClasses = true

[minify]
disableXML = true
minifyOutput = true

[params]
env = "production"
title = "ExampleSite"
description = "ExampleSite description"
keywords = [ "Blog", "Portfolio", "PaperMod" ]
author = "Me"
images = [ "<link or path of image for opengraph, twitter-cards>" ]
DateFormat = "January 2, 2006"
defaultTheme = "auto"
disableThemeToggle = false
ShowReadingTime = true
ShowShareButtons = true
ShowPostNavLinks = true
ShowBreadCrumbs = true
ShowCodeCopyButtons = false
ShowWordCount = true
ShowRssButtonInSectionTermList = true
UseHugoToc = true
disableSpecial1stPost = false
disableScrollToTop = false
comments = false
hidemeta = false
hideSummary = false
showtoc = false
tocopen = false

  [params.assets]
  favicon = "<link / abs url>"
  favicon16x16 = "<link / abs url>"
  favicon32x32 = "<link / abs url>"
  apple_touch_icon = "<link / abs url>"
  safari_pinned_tab = "<link / abs url>"

  [params.label]
  text = "Home"
  icon = "/apple-touch-icon.png"
  iconHeight = 35

  [params.profileMode]
  enabled = false
  title = "ExampleSite"
  subtitle = "This is subtitle"
  imageUrl = "<img location>"
  imageWidth = 120
  imageHeight = 120
  imageTitle = "my image"

    [[params.profileMode.buttons]]
    name = "Posts"
    url = "posts"

    [[params.profileMode.buttons]]
    name = "Tags"
    url = "tags"

  [params.homeInfoParams]
  Title = "Hi there 👋"
  Content = "Welcome to my blog"

  [[params.socialIcons]]
  name = "x"
  url = "https://x.com/"

  [[params.socialIcons]]
  name = "stackoverflow"
  url = "https://stackoverflow.com"

  [[params.socialIcons]]
  name = "github"
  url = "https://github.com/"

[params.analytics.google]
SiteVerificationTag = "XYZabc"

[params.analytics.bing]
SiteVerificationTag = "XYZabc"

[params.analytics.yandex]
SiteVerificationTag = "XYZabc"

  [params.cover]
  hidden = true
  hiddenInList = true
  hiddenInSingle = true

  [params.editPost]
  URL = "https://github.com/<path_to_repo>/content"
  Text = "Suggest Changes"
  appendFilePath = true

  [params.fuseOpts]
  isCaseSensitive = false
  shouldSort = true
  location = 0
  distance = 1_000
  threshold = 0.4
  minMatchCharLength = 0
  limit = 10
  keys = [ "title", "permalink", "summary", "content" ]

[[menu.main]]
identifier = "categories"
name = "categories"
url = "/categories/"
weight = 10

[[menu.main]]
identifier = "tags"
name = "tags"
url = "/tags/"
weight = 20

[[menu.main]]
identifier = "example"
name = "example.org"
url = "https://example.org"
weight = 30

[markup.highlight]
noClasses = false

```

### Test Hugo site
```shell
hugo server -t PaperMod
```

Now the page appears empty beacuse we haven't put our blog files in it
To do that just copy the *posts* folder made in Obsidian and paste it in the content folder of your hugo site

### Add Properties 
We can add properties to the markdown file by toggling to source mode and adding 
```yml
---
title: My Blog Pipeline
date: 2024-11-28
draft: false
tags:
-blog
---
```
to the starting of our file.

*We can also create a template for our blogs by using Templater plugin available in Obsidian*

### Attachments
Now by default Obsidian stores attachments in a specific folder called attachments but with that its complicated to fetch images every time from that folder.
Rather what we can do is 
![](../attachments/Pasted%20image%2020241128172509.png)
change the default location for attachments to "In subfolder under current folder" and this will store our attachments in an "attachment" folder in our current folder i.e., where the current file being edited is stored.
And then to get proper links for these images
![](../attachments/Screenshot%202024-11-28%20101615.png)
and then paste a photo in the file and put "../" before the photo address.

### Deploy 
- First build the Hugo site :
	`hugo`
- Now you can push this code to a github repo and deploy the website on platforms like vercel, cloudflare pages, etc.
 *In root directory put "public"*

### Now, lets see how to automate some things
- First we are going to create a github action that automatically builds our hugo site when we push code on it
```yaml
name: Build and Deploy Hugo Site

on:
  push:
    branches:
      - master  # Trigger on push to master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout Repository
      uses: actions/checkout@v4

    - name: Install Hugo
      run: |
        HUGO_VERSION="0.139.2"  # Specify the Hugo version
        wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz
        tar -xvzf hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz
        sudo mv hugo /usr/local/bin/hugo
        rm hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz

    - name: Build Hugo Site
      run: |
        rm -rf public  # Clean previous builds
        hugo           # Build the Hugo site

    - name: Deploy to Deploy Branch
      env:
        GITHUB_TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
      run: |
        git config user.name "GitHub Actions Bot"
        git config user.email "github-actions[bot]@users.noreply.github.com"
        git add .
        git commit -m "Deploy site $(date +'%Y-%m-%d %H:%M:%S')"
        git push --force "https://${{ secrets.PERSONAL_ACCESS_TOKEN }}@github.com/${{ github.repository }}" HEAD:master

```
For this you will need a Personal_Acess_Token from https://github.com/settings/personal-access-tokens 
Now add the Token at `https://github.com/<username>/<repository>/settings/secrets/actions`
in Repository secrets

- Now create a .gitignore file and write 'public' in the file
```
#.gitignore
public
```
*this is done because it later may lead to merge issues*

*from inside the obsidian-blog folder*
*rsync -av --delete ../../Obsidian/Notes/posts/ ./content/posts*