+++
categories = ["Tutorials", "Golang", "GitHub Pages", "Git"]
date = "2015-06-29T18:28:53+02:00"
tags = ["Hugo", "GoLang", "Tutorials", "GitHub Pages"]
title = "Hello HuGO - Create a Hugo Blog on GitHub Pages via Git"
draft = "false"

+++

After a long time of debating (read: procrastinating) about creating a blog, here it is. Its purpose is dual – first, document my adventures in Go & Swift (among other tools of the trade) as a way to reinforce concepts I am learning and implementing into projects. Secondly, it is a great platform to share my perspective on miscellaneous subjects, in which (I hope) can spark meaningful discussion from all viewpoints.

As my first post, I’ve decided to pop open the hood of my brand new blog and explain its workings. My blog is powered by [Hugo](http://gohugo.io), the static site generator written by [Steve Francia](http://github.com/spf13/hugo). And since it is written in Go, you can pretty much be assured of its speed, with the ability to render pages in milliseconds. Since I am changing my current web host from S3 to GitHub Pages, my workflow has changed and this tutorial reflects this update (with the absence of the task manager I use, [Grunt](http://gruntjs.com), which I will detail in a future post). It is written so that anyone can understand (although the [documentation](http://gohugo.io/overview/introduction) is clear and the [discussion](http://discuss.gohugo.io) section is extensive and helpful) but I am assuming that you are comfortable with using the command line and have some familiarity with git, the open source version control system.

## Why Hugo? 
I initially considered [Jekyll](http://jekyllrb.com/), but chose against it (speed). The ability to write my blog posts in markdown crossed off other options.
With Hugo, organization is straightforward, with the paths of folders remaining the same as a url; for example, the file in content/holla/back/girl.md is rendered as \<site>/holla/back/girl when live. And if that is not enough, Hugo comes with a webserver that automatically updates as you alter content on the site.  

## Setting it up
First you're going to need to install it. I will assume you are working from OSX. If you have [Homebrew](http://www.brew.sh), you can simply do `$ brew install hugo`.

#### Workflow 
1. Create a repo with your username followed by github.io. as so:`github_username.github.io`. This is the name of the repo and will be the url of your site. 

2. Now create a local instance of your repo under the same name and using the command line, cd to create a new site in this current directory: `$ hugo new site` or you can specify the path when creating the new site, like so: `$ hugo new site /path/to/folder`.

3. You then will have to initialize the git repo with `$ git init` and add your remote 
`$ git remote add origin git@github.com:<github_username>/<github_username>.github.io.git`. Now we are ready for a new blog post.  

4. A new blog post is just as simple, `$ hugo new post/hello.md`. Make sure you set draft to false so that the post appears.

5. `$ hugo` command builds the site and you should have a complied public directory with your site.

6. Now we must do an `$ echo 'public' >> .gitignore` so that Git may ignore your public directory.

7. `$ git checkout -b source` will switch you out of the master branch and create a new branch (source). As you might have surmised by now, we will use this branch to keep our source code for our site, while the master branch will hold the actual site (in the public directory).  
 
8. Add, commit and push! `$ git add -A` followed by `$ git commit -m 'Init Commit'`and `$ git push origin source`.

9. Now we simply cd into the public folder. `$ git init` followed by add, commit and push (Push is done with `$ git push origin master`). Now when you direct your browser to github_username.github.io, you should see your site. 

#### Themes!
Whatever theme you feel best matches your taste, you can clone and use. I am using [GreyShade](https://github.com/cxfksword/greyshade). `$ git clone https://github.com/cxfksword/greyshade.git theme/greyshade`. Here I placed the folder and name of the theme. To test it out, run the Hugo webserver `$ hugo server -w`. If all is well, tell Hugo to build the site by running `$ hugo`. Now you should have a compiled public directory with your theme. 

#### Last Thoughts
Initially, customizing the theme was a bit tedious since I had to compile the Sass files before I could see the changes live on the browser. I recommend a task manager to automate these things, as mentioned earlier. Overall, once you understand the first few steps, you’ll be quite amazed of the simplicity and power of Hugo. The switch has certainly done good for my needs. * *Warning: minor rant approaching*. * In terms of enabling comments, the theme I am currently using is designed to use [Disqus](https://disqus.com/) but I have chosen to do away with that completely. With governments worldwide violating our privacy in such obscene ways that threaten our human and civil rights, should we just accept the massive exploitation of our data and the rapid erosion of what little privacy we might have left as the new norm? Let’s hope not. Meanwhile, lets create awesome sites with Hugo and fight the good fight.