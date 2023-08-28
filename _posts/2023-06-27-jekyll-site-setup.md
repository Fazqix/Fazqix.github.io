---
layout: post
title: "Jekyll | A Static Site Generator"
date: 2023-08-27 12:00:00 -500
categories: Website
tags: jekyll website site generator
pin: true
image:
    path: /assets/img/headers/Jekyll-Site-Setup-Header.webp
---

**Jekyll** is one of the most popular static site generators that transform plain text into beautiful static websites and blogs. It is written in Ruby and it is powered by GitHub Pages, which means that you can host your Jekyll site for free on GitHub. Jekyll has a simple workflow:

- You write your content in plain text files using Markdown, HTML, or Liquid.
- You organize your files into folders and subfolders according to your site structure.
- You define your site settings in a configuration file called `_config.yml`.
- You create templates for your pages, posts, and layouts using Liquid tags and filters.
- You run `jekyll build` to generate your static site files in a folder called `_site`.
- You run `jekyll serve` to preview your site locally at `http://localhost:4000`.
- You deploy your site to GitHub Pages or any other hosting service.

Jekyll sites are commonly used for documentation sites, personal or professional blogs, or an event sites. These sites are not limited to these specific themes but are the easiest to set up through the initial layout.

▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔
## Windows Walkthrough

Steps Included in this Walkthrough :

-   Installing Ruby
-   Installing Jekyll
-   Installing Bundler
-   Creating the Site's GitHub Repository

Prerequisites Required :

-   You must already have [GIT](https://gitforwindows.org/) installed.
-   You must already have your preferred text editor installed. I recommend using [Visual Code](https://code.visualstudio.com/) if you do not have one in mind.

### Installing Ruby

Visit the [Ruby Site](https://rubyinstaller.org/) and download the latest Ruby version with DevKit.

<details>
<summary><strong>Screenshots</strong></summary>

</details>

Once the initial installation process is completed, a command prompt window will open asking for MSYS2 

<details>
<summary><strong>Screenshots</strong></summary>

</details>

Once the installation process in completed, it is recommended that you reboot your computer. Once the computer reboots, you can confirm that Ruby and Gem were properly installed by opening up command prompt and running these two commands :

```bat
ruby -v
```

```bat
gem -v
```

### Installing Jekyll

Within the Ruby Command Prompt, run this command to install Jekyll :

```bat
gem install jekyll
```

To check if Jekyll was correctly installed, the version should pop up when running this command :

```bat
jekyll -v
```

<details>
<summary><strong>Screenshots</strong></summary>

</details> 

### Installing Bundler

Within the Ruby Command Prompt, run this command to install Bundler :

```bat
gem install bundler
```

To check if Bundler was correctly installed, the version should pop up when running this command :

```bat
bundler -v
```

<details>
<summary><strong>Screenshots</strong></summary>

</details> 

### Creating the Site's GitHub Repository



