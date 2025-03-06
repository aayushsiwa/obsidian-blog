---
title: Building a Dynamic GitHub Portfolio with AI and Automation (with Code)
date: 2025-01-10
description: I embarked on a project to create a dynamic and informative portfolio for my GitHub repositories. My goal was to automatically generate concise and engaging descriptions for each project, and then store those descriptions in a JSON file, ready to be used on a portfolio website.
summary: I embarked on a project to create a dynamic and informative portfolio for my GitHub repositories. My goal was to automatically generate concise and engaging descriptions for each project, and then store those descriptions in a JSON file, ready to be used on a portfolio website.
tags:
  - blog
  - github
  - actions
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
## Building a Dynamic GitHub Portfolio with AI and Automation

We embarked on a project to create a dynamic and informative portfolio for our GitHub repositories. The goal was to automatically generate concise and engaging descriptions for each project, and then store those descriptions in a JSON file, ready to be used on a portfolio website. This project involved a blend of GitHub API interaction, AI-powered content generation, and automation through GitHub Actions. Here's a behind-the-scenes look at how we built it, including key code snippets and helpful links.

**Phase 1: Gathering Repository Data**

Our first step was to gather the necessary data from our GitHub repositories. We wanted to extract information like:

- Repository name
- Languages used
- Topics
- Homepage URL

To achieve this, we utilized the GitHub API. Here's a snippet of our Python script:

Python

```
import requests

def get_repo_data(github_owner, github_token, topic="project"):
    headers = {'Authorization': f'token {github_token}'}
    api_url = f"https://api.github.com/search/repositories?q=user:{github_owner}+topic:{topic}"
    response = requests.get(api_url, headers=headers)
    response.raise_for_status()
    return response.json()["items"]
```

- **GitHub API Documentation:** [https://docs.github.com/en/rest](https://www.google.com/url?sa=E&source=gmail&q=https://docs.github.com/en/rest)

**Phase 2: AI-Powered Description Generation**

Next, we needed to generate compelling descriptions for each repository. This is where the power of AI came into play. We integrated the Gemini API into our Python script.

We crafted prompts that included the repository name, languages, and topics, and then sent them to the Gemini API. Here's how we used the Gemini API:

Python

```
import google.generativeai as genai

def generate_description(repo_name, languages, topics, api_key):
    genai.configure(api_key=api_key)
    model = genai.GenerativeModel('gemini-2.0-flash')

    prompt = f"""
    Generate a concise one-paragraph description for a GitHub repository.

    Repository Name: {repo_name}
    Languages Used: {', '.join(languages)}
    Repository Topics: {', '.join(topics)}

    Create a description that highlights the purpose, technologies, and topics.
    """
    response = model.generate_content(prompt)
    return response.text
```

- **Google AI Studio (Gemini API):** [https://ai.google.dev/](https://www.google.com/url?sa=E&source=gmail&q=https://ai.google.dev/)
- **Google Generative AI Python SDK:** [https://ai.google.dev/tutorials/python_quickstart](https://www.google.com/url?sa=E&source=gmail&q=https://ai.google.dev/tutorials/python_quickstart)

**Phase 3: Structuring the Data**

We wanted to present the repository data in a specific JSON format, suitable for use in a portfolio website. This format included:

- `title`: Repository name
- `description`: Generated description
- `imgSrc`: Link to a screenshot (constructed dynamically)
- `githubLink`: Repository URL
- `liveLink`: Homepage URL

We modified our Python script to structure the data in this format and then write it to a `projects.json` file.

Python

```
import json

def create_json(repos, api_key, github_owner, github_token, output_file="projects.json"):
    descriptions = []
    for repo in repos:
        # ... (extract languages, topics, homepage) ...
        description = generate_description(repo['name'], languages, topics, api_key)
        img_src = f"https://raw.githubusercontent.com/{repo['full_name']}/master/screenshot.png"
        descriptions.append({
            "title": repo['name'],
            "description": description,
            "imgSrc": img_src,
            "githubLink": repo['html_url'],
            "liveLink": repo.get("homepage", None)
        })
    with open(output_file, "w") as f:
        json.dump(descriptions, f, indent=4)
```

**Phase 4: Automating with GitHub Actions**

To make this process truly dynamic, we automated it using GitHub Actions. We created a workflow that would trigger every time a push was made to any repository in our GitHub profile.

Here's a snippet of our GitHub Actions workflow file (`.github/workflows/generate_descriptions.yml`):

YAML

```
name: Generate Repo Descriptions

on:
  push:
    branches:
      - '*'

jobs:
  generate_descriptions:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.9'

      - name: Install dependencies
        run: pip install requests google-generativeai

      - name: Generate descriptions
        env:
          GEMINI_API_KEY: ${{ secrets.GEMINI_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
        run: |
          python main.py --api_key $GEMINI_API_KEY --github_owner ${{ github.repository_owner }} --github_token $GITHUB_TOKEN

      - name: GitHub Commit & Push
        uses: actions-js/push@v1.5
        with:
          github_token: ${{ secrets.PERSONAL_ACCESS_TOKEN }}
          message: "Update repo descriptions"
          branch: master
          directory: .
          force: true
```

- **GitHub Actions Documentation:** [https://docs.github.com/en/actions](https://www.google.com/url?sa=E&source=gmail&q=https://docs.github.com/en/actions)1
- **GitHub Actions Secrets:** [https://docs.github.com/en/actions/security-guides/encrypted-secrets](https://www.google.com/url?sa=E&source=gmail&q=https://docs.github.com/en/actions/security-guides/encrypted-secrets)
- **actions-js/push:** [https://github.com/actions-js/push](https://www.google.com/url?sa=E&source=gmail&q=https://github.com/actions-js/push)

**Challenges and Solutions**

- **Authentication:** We used a personal access token (PAT) with the `repo` scope and stored it as a GitHub secret.
- **Error Handling:** We added `try...except` blocks and input validation to our Python script.
- **Image Link Construction:** We used f-strings to dynamically create the image links.

**Outcome**

The project culminated in a fully automated system that generates and updates a JSON file containing descriptions of our GitHub projects. This JSON file can now be used to power a dynamic portfolio website, showcasing our work in a consistent and engaging manner.