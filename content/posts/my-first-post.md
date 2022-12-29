---
title: "Creating a site using Hugo"
date: 2022-12-11T18:28:37+01:00
tags:
  - Go
  - Hugo
---

## Introduction

Hello and welcome! This is the first post in this blog. This blog is meant as a way for me to keep track of all the little projects I want to do, and how those projects went. 

## Hugo

This first project was setting up this website with [Hugo](https://gohugo.io), so for my first blog I might as well share how the experience has been.
While a lot of people got their start in software engineering through making websites, I've not come down this path so this is my experience unburdoned by any prior website-making experience. Hugo is a tool to generate a static website through markdown pages. You write your blogposts and general pages in the markdown format, and it generates all the html/css etc. I really like this idea, no live updates or cookies or javascript anything to maintain or worry about. 

It's honestly been as easy as one could resonably expect, I had to install 'go' which was a 0.5 gig dependency and I could install hugo through [Homebrew](https://brew.sh). I roughly followed the [quickstart tutorial](https://gohugo.io/getting-started/quick-start/) offered by Hugo themselves.

## Templates

After running `hugo new site quickstart` you've got the project setup all complete.
It was a lot of fun being able to immidiately look through all the pretty [templates](https://themes.gohugo.io) available without really having to do a lot of reading. Installing them was a bit odd, since you have to `git clone` them straight into the generated themes folder. I guess I'm a bit spoiled with npm, where it will automatically download dependencies to the correct folder. It's not a big deal at all, but it's easy to forget. I've seen people solve this problem by slightly modifying their git clone to `git clone linktogitrepo themes/nameoftemplate`. Which will automatically 'install' the project in the themes folder with the name of your choosing without having to switch directories in your terminal.

It's honestly a minor inconvenience but it got me stuck on a weird error when I accidentaly tried to start the server through `hugo start` in the `themes` folder. This gives you the following error:

    Error: Unable to locate config file or config directory. Perhaps you need to create a new site.
    Run `hugo help new` for details.
    
It did take me a bit to figure out that my site was fine, I was just in the wrong folder.

After installing a great looking theme, you can set it in the `config.toml` file. The first theme I tried was using `.yaml` instead which seemed fine enough for me first. But as far as I can see, it seems to be the old way of doing things and after getting stuck trying to change an icon, I just looked for a different theme that did 'support' a toml config through their configuration. I currently landed on the [hello-friend-ng](https://themes.gohugo.io/themes/hugo-theme-hello-friend-ng/) theme.

This could all change in the future though. The great thing about the theme system is that you can change the whole look of the website without having to change the individual pages. All in all going from nothing to writing this first entry took me about one weekend. If you give it more priority than I have though, you could have your first website set up in a day. 