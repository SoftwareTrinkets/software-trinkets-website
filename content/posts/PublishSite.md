---
title: "Publishing a website created through Hugo"
date: 2022-12-28T22:55:37+01:00
draft: true
---

After creating this site I've been tweaking what I want it to look like, created a logo, finished a second post so it wouldn't be that empty when I launched it. But after all that procrastination it was time to get it online. I didn't think this would be too big a deal, but honestly, it took me way longer than expected.

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
After checking that the domain was still available I went to the organisation that manages names in the Netherlands. https://www.sidn.nl 
Something that's also good to keep in mind, there's cheap options where you get both hosting and they'll get a domain name for you. However, after that you don't own the domain name, and if you want to switch to another service you're basically out of luck.

## Getting the site on GitHub Pages

After getting the domain, I thought something low-stakes would be to get the site on 'GitHub Pages' page first. This, of course, also took longer than expected. 
Getting a page on [GitHub pages](https://pages.github.com) is actually pretty easy. Once you have the website comitted to github you can go into settings and set a branch to go to the 'GitHub Pages' page. 

So, running `hugo` in your project folder will make it create your static website in the `public` folder. You can then put this on a seperate branch and set the GitHub Pages page to start there. 

HOWEVER

For me, I thought this would be it, set it and forget it. But nothing worked, I only saw HTML and a bunch of 404 that it couldn't find files that should just be there. I could see them in the branch, but they couldn't be found for some reason. 

## Set the baseURL in the config.toml

I didn't really understand what this setting was for when I was building the website, but I've found out the hard way. If this isn't set to the url that you're on it will not work. It will only show you some static html and nothing else. 
For GitHub Pages you need the domain and the repo name, so in my case that is:
    
    baseURL = "https://softwaretrinkets.github.io/software-trinkets-website/"

## The branch thing isn't the best

I can't recommend what I've done here, currently I just copy over the public folder to a specific github pages branch and that's what it uses. It's quite tedious and I'm considereing making an action for it. 