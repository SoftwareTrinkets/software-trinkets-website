---
title: "Publishing a website created through Hugo"
date: 2022-12-28T22:55:37+01:00
tags:
  - Hugo
  - GitHub Actions
  - Domain
draft: true

---

After creating this site I've been changing what I want it to look like, created a logo, finished a second post so it wouldn't be that empty when I launched it. But after all that procrastination it was time to get it online. I didn't think this would be too big a deal, but honestly, it took me longer than expected.


## The plan

So the plan was the following:

- Think of a name.
- Register a domain name.
- Use an email address connected to that domain name to create a new GitHub page (since my normal email is already attached to an account there)
- Create a new GitHub repo with all this code on it.
- Set up GitHub Pages with the generated site
- Route this to my custom domain

## The reality

I've never registered a domain name before, I know, embarrassing. Better late than never though, but it was still a bit daunting. Apparently you can't just google or check the domains, just in case something is watching for that and getting it before you can. So you have to do a terminal check for the domain you want, see if it returns anything. 
`TODO: The way to check here`
After checking that the domain was still available I went to the organisation that manages names in the Netherlands. https://www.sidn.nl. I'm going for a .nl domain, since that's very cheap and good enough for my purposes. The first year of this domain will cost me roughly 1 Euro, hard to beat that kind of price. After that it's about 10. 
Something that's also good to keep in mind, there's cheap options where you get both hosting and they'll get a domain name for you. However, after that you don't own the domain name, and if you want to switch to another service you're basically out of luck.

## Getting the site on GitHub Pages

After getting the domain, I thought something low-stakes would be to get the site on 'GitHub Pages' page first. This, of course, also took longer than expected. 
Getting a page on [GitHub pages](https://pages.github.com) is actually pretty easy. Once you have the website comitted to GitHub you can go into settings and set a branch to go to the 'GitHub Pages' page. 

So, running `hugo` in your project folder will generate your static website in the `public` folder. You can then put this on a seperate branch and set the GitHub Pages settings to start there. 

HOWEVER

For me, I thought this would be it, set it and forget it. But nothing worked, I only saw raw HTML and a bunch of 404 that it couldn't find files that should just be there. I could see them in the branch, but they couldn't be found for some reason. 

![404](PublishedSite404.png)

## Set the baseURL in the config.toml

I didn't really understand what this setting was for when I was building the website, but I've found out the hard way. If the baseURL isn't set to the url that the website is on, it will not work. It will only show you some static html and nothing else. 
For GitHub Pages you need both the domain and the repo name, so in my case that is:
    
    baseURL = "https://softwaretrinkets.github.io/software-trinkets-website/"

After I got that right, I was able to see the site correctly. I did get into trouble with my themes folder dissapearing when I switched to put a new version on the gh-pages branch. Which leads us directly to the next point.

## Automating 

I honestly feel like I can't be seen with a website that requires me to manually move a folder to a branch, that just doesn't feel right. So I moved straight on to automation, there already is some automation in the project which was generated for the GitHub Pages. 
Deployment seems like too big a word for copying over a folder from the main branch to the gh-pages branch, but it seems like exactly the kind of thing that's kind of tricky to do manually but super easy to do with automation. 

I found this thread: 
https://github.com/actions/checkout/discussions/405

Which is discussing the exact thing I want to do here. In case this is the first time you're dealing with automation, here's what you do. First, go to the Actions tab in GitHub:

![Automation1](Automation-1.png)

On this page, you can create a new workflow by clicking **New workflow**

![Automation2](Automation-2.png)

Next, select **set up a workflow yourself**, this will create a `main.yml` for your automation

![Automation3](Automation-3.png)

In this file, put I just put the automation from the [github actions thread](https://github.com/actions/checkout/discussions/405).

![Automation4](Automation-4.png)


    name: Deploy Site

    on:
    workflow_dispatch:
    push:
        paths-ignore:
        - '.github/**' # Ignore changes towards the .github directory
        branches:
        - main # Only trigger on the development branch

    jobs:
    build:
        runs-on: ubuntu-latest
        steps:
        - name: Perform Checkout
        uses: actions/checkout@v2
        - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_dir: ./public
            publish_branch: gh-pages

After committing this straight to `main`, we're all ready to run it! This automation should copy the `public` folder (`publish_dir: ./public`), which will then trigger the ealier GitHub Pages automation which serves everything to the https://softwaretrinkets.github.io/software-trinkets-website/ site

For me, this worked, huzzah! Now I only have to run `hugo` locally and push the changes to main for it to automatically pick them up and deploy them through the gh-pages branch.
But we can take it further than this... Wouldn't it be more impressive if we didn't even have to run `hugo` locally?

Since Hugo is a pretty big static site generation framework, someone already made a GitHub action for this! [Look at it!](https://github.com/marketplace/actions/hugo-setup) So next we can basically copy this and use it for our own automation. I only needed minor changes to make it do what I needed it to do. I updated the hugo version to the one I was using locally, and added `publish_branch: gh-pages` at the bottom to make the changes go to that branch specifically:

    name: Run Hugo and deploy to branch

    on:
    workflow_dispatch:
    push:
        paths-ignore:
        - '.github/**' # Ignore changes towards the .github directory
        branches:
        - main # Only trigger on the development branch

    jobs:
    deploy:
        runs-on: ubuntu-22.04
        concurrency:
        group: ${{ github.workflow }}-${{ github.ref }}
        steps:
        - uses: actions/checkout@v3
            with:
            submodules: true  # Fetch Hugo themes (true OR recursive)
            fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

        - name: Setup Hugo
            uses: peaceiris/actions-hugo@v2
            with:
            hugo-version: '0.108.0'
            # extended: true

        - name: Build
            run: hugo --minify

        - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            if: ${{ github.ref == 'refs/heads/main' }}
            with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_dir: ./public
            publish_branch: gh-pages

And **BAM**, now I only have to push my content to the repo, and it will automatically build a new website, and deploy it to GitHub Pages. Even though it wasn't that much work to set up this automation, it'll save me so much time now that I don't have to copy over the public folder to a branch myself. With the added benefit that I don't even have to build locally anymore. 