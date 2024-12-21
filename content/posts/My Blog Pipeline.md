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
- Now you canasdasdadas push this code to a github repo and deploy the website on platforms like vercel, cloudflare pages, etc.
 *In root directory put "public"*


test5