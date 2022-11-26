---
title: "Migrating Blog to Hugo"
date: 2022-11-28T11:00:00+01:00
author: Maarten Van Driessen
type: post
url: /migrating-blog-to-hugo/
categories:
  - Random
tags:
  - blog
  - hugo
---

Ever since I started my blog back in 2016, it's been running on wordpress. It took me a while before I got to a setup that I liked. Back when I started, I had chosen a free hoster.
This quickly presented issues as I was not satisfied with the speed of my blog. The blog was moved around 2 or 3 more times before I ended up going to my current hoster.

Over the years, I've had several issues with managing my wordpress instance. Security issues with plugins or php, upgrading php version, plugins no longer being developed, ...
I've grown tired of having to manage wordpress. I just want to write content and publish it.

## Meet hugo
Lately, I saw quite a few bloggers making the transition to [Hugo](https://gohugo.io) so I thought I would take a look. What I found is that there's a lot to like!
Hugo is a framework for building blazing-fast static websites. All content is written in Markdown and generated upon save or commit.

Because it's a static website, there's no plugins to manage, no maintenance to be done, no security issues to patch, easy!
What really makes it very cool is that you can just put your markdown files on github and have the website completely rebuild with every commit to main! I know that might seem plain to most people, but I still find that pretty cool.

As a hoster I chose to go with [Netlify](https://www.netlify.com), I read good feedback about them and their free tier would be more than enough for what I'm doing now.

## Migration process
The migration process is fairly straightforward, I got a lot of pointers from [Christian Mohn](https://twitter.com/h0bbel)'s blog [post](https://vninja.net/2018/07/22/migrating-from-wordpress-to-hugo/). 

The first thing you can do is download and install Hugo on your local machine, then pick a [theme](https://themes.gohugo.io/) you like and start changing the content.

In essence, you make an export of all your wordpress posts and then fix any broken links and weirdness that happens during the export.
Special characters got a bit messed up during the export so what I did was:
* replace ```&#8230;``` with ...
* replace ```&#8222;``` with "
* replace ```&#8220;``` with "
* replace ```&nbsp;``` with a space
* replace ```&lt;``` with <
* replace ```&gt;``` with >

A lot of my posts also contain code examples. In the export these examples were put in between ``` <pre> ``` tags. So I went through each post and replaced those pre tags with 3 backticks ` to get the same result.

Finally, what consumed the most time was getting all the images replaced. Wordpress generates multiple versions of the image on different resolutions. I haven't quite gotten around to fully implementing that on Hugo. So what I did was just use the original png or jpg file and replaced the image link with the following
```
    { {< figure src="./images/img.png" alt="alt text">} }
```
Remove the extra space between the curly braces. I had to put it there or the code wouldn't render properly...

### Content layout
There's several ways you can organize your content. You could just dump all your MD files in the content/posts folder and all your images in the static folder and call it a day. This presents a few issues, though.
The folders quickly become very messy and hard to work with. A better solution is to use [page bundles](https://gohugo.io/content-management/page-bundles/).

Page bundles are really easy. You just organize all your content in folders, that have the name of the post, and rename your markdown files to ```index.md```.
You can go even further and use those folders to organize your content even further. I'm going to look into this some more. I didn't do it now because it would break a lot of links.

With page bundles, a markdown file can only access the content that's in the same folder. This is really convenient for images, you just create a folder called ```images``` and put all your image files in there. This allows you to have duplicate file names and just have your content organized better, as you can see in the example below.

{{< figure src="./images/hugo-folder.png" alt="Hugo Folder">}}

## Todo list
I've spent a fair bit of time getting the site live the way that it is now and I'm happy with it! But I'm not entirely done yet, there's still a few things that are missing.
* New about page needs to be written
* Search is missing
* I might change the theme, not entirely convinced this is the best way to read the content
* Favicon is missing
* Page updates should be shown
* Other random stuff I can come up with

I hope you found this post useful and that you like the amazing speed of the reworked blog. If you find anything broken or have any suggestions, please hit me up on [twitter](https://twitter.com/mvandriessen)!