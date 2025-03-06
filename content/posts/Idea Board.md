---
title: Idea Board
date: 2025-01-02
description: A basic web app "Idea Board" to store our ideas
summary: A basic web app "Idea Board" to store our ideas
tags:
  - blog
  - react
  - vite
  - typescript
  - firebase
  - idea-board
author: Me
showToc: true
TocOpen: true
draft: false
hidemeta: false
comments: false
disableHLJS: false
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: false
ShowPostNavLinks: false
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: false
cover:
  image: <image path/url>
  alt: <alt text>
  caption: <text>
  relative: false
  hidden: true
---
## Step 1: Initiate a new vite-app
```bash
npm create vite@latest
```
This initializes a new vite app
Here we are going to use React and TypeScript
so select "React" and then select "TypeScript"
then go into the app directory and run 
```bash
npm install
```
Now lets use Tailwind CSS in our project
#### Install Tailwind CSS
Install `tailwindcss` and `@tailwindcss/vite` via npm.
```bash
npm install tailwindcss @tailwindcss/vite
```
#### Configure the vite plugin
Add the `@tailwindcss/vite` plugin to your Vite configuration.
```ts
import { defineConfig } from 'vite'
import tailwindcss from '@tailwindcss/vite'
export default defineConfig({
	plugins: [
	    tailwindcss(),
	],
})
```
#### Import Tailwind CSS
Add an `@import` to your CSS file that imports Tailwind CSS.
Note: here we will remove App.css
and add this import in index.css
```css
@import "tailwindcss";
```
## Step 2: Remove the base app configs
After removing the base configs:

App.tsx
```tsx
function App() {
  return (
    <></>
  )
}
export default App
```

index.css
```css
@import "tailwindcss";
```

As said above we can remove `src/App.css`, `src/assets/react.svg` and `public/vite.svg`
And we can change the title in index.html

## Step 3: Now let's create a basic webpage
create a nav bar that shows options to login and implement authentication
For authentication and database, we will be using Google Firebase
So create a project in firebase and let's get going.