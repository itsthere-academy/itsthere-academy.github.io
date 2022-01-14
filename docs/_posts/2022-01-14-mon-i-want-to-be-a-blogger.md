---
layout: post
title:  "Mom! I want to be a blogger!"
date:   2022-01-14 6:00:00 +0100
categories: astro org
---
Blogging on blogging. Let’s talk about technical aspects of blogging. You may want to Get yourself a cope of coffee. It’s a longer one.

There’s plenty of ways I could have setup this blog. Put it on [Medium](http://medium.com/), use free tier [WordPress](https://wordpress.com/) or even [host WordPress somewhere in the cloud (like AWS)](https://aws.amazon.com/getting-started/hands-on/launch-a-wordpress-website/).

In fact I was really close to go ahead with Medium. I appreciate the balanced aesthetics and ease of use. Looks and works fantastic both in browser and dedicated mobile app. Browsing Medium blogs is such a (visual) treat!

There’s a caveat though - to take the full advantage of the platform, you should consider paid Medium membership. It’s not that expensive, but makes it more of a commitment. The platform uses the money not only to fund itself, but to (financially) reward the best authors! Even though not required, you may be tempted to limit your audience to premium (paying) platform users. In other words, Medium is more of an ecosystem than blogging platform alone. The choice is yours.

WordPress on the other hand is one of the most powerful platforms in the observable universe (this was dry, I know). It’s a skill on its own. Hosting and/or extensions building know-how is a career! Impressive, but maybe some other day - too much of an overhead for me right now.

So I ended up with GitHub Pages. 

Huh? Wasn’t GitHub for sharing code and maybe documentation for the code?
Yes, you are right. What is maybe less know - GitHub lets you host a simple web page for the project, or a blog. Will do it for free (within some limits). If you do it right, the page/blog is reachable under nice github.io subdomain. Have a look at [itsthere-academy.github.io](https://itsthere-academy.github.io) - looks familiar?

Especially the last one was critical for what I was looking for. But... maybe I should have started with what I was looking for. In no particular order:
1. Ability to move the blog to other platform without changing the URL (the address).
2. Flexible on the content I can post - images, dynamic content (i.e. [Plotly](https://plotly.com/) graphs that need JavaScript), pieces of code.
3. Budget matters. Not insisting on being free, but surely the cheaper the better.

Let’s cross check with my wish list:
1. I can register a domain and alias to blog’s GitHub subdomain. If I want to move the blog to e.g. WordPress, I’m free to do so. Check.
2. GitHub Pages will host pretty much any HTML content, which includes JavaScript scripts. [Plotly](https://plotly.com/) (and similar) work like a charm. As a bonus, GitHub is probably most common way to share code. Check.
3. It’s free within limits I’m very unlikely to breach. Check.

If you understood nothing of it, take it as the first warning. GitHub Pages is not for everyone. This portal is for programmers and so are GitHub Pages. 

What do I mean by this? Get ready to use git, as your blog is nothing but a Git repo - like mine. Get ready to dig into Jekyll (Markdown to HTML processor) manuals. Get ready to dig into HTML and CSS for even tiny template customizations - see the logo at the top of the page? Unless this is what you’ve been doing before, the learning curve can be steep. Make sure this is where you want to spend your time.

Don’t get me wrong - GitHub did a great job simplifying the experience, but it’s still GitHub. Which is good :)

So how does my blog really work?

First I bought a domain - *itsthere.academy*. I didn’t spend too much time on it, did it via AWS. AWS offers pretty comprehensive DNS support + enables me to use AWS for non-blog components when the time comes. The domain was $11 per year + tax. Yes, you can buy a cheaper one for $4 or $5, but there’s a catch - for some reason the cheaper ones won’t let you hide your personal information (contact) from being publicly visible in whois database. You didn’t think about this, huh?

Now to GitHub. As I said before, GitHub Page is nothing but a repo. You push your posts in Jekyll acceptable form, GitHub runs Jekyll processor as an Action (build step). If you already have GitHub account... good for you, but you still may want to create a new one, named exactly like your blog. 

Let me explain.

Any GitHub account can have multiple repos. Each repo can be turned into GitHub page, but only repo named *account_name.github.io* can be reached as *account_name.github.io* page. Otherwise it gets published as *account_name.github.io/repo*. 

Why this is important? Let’s jump to DNS configuration.

After some deliberation, I decided the blog should be published under a subdomain, as *[blog.itsthere.academy](https://blog.itsthere.academy)*. The fact GitHub exposes the blog as [itsthere-academy.github.io](https://itsthere-academy.github.io) (with no path component, e.g. *piotr-szukalski.github.io/itsthere-academy*) means I can simply configure AWS Route 53 with CNAME record pointing at [itsthere-academy.github.io](https://itsthere-academy.github.io). If I ever want to move out from GitHub Pages, I only need to reconfigure my DNS and done. No redirects from old GitHub location, no “the page has moved” and so on. Perfect!

![DNS output for blog.itsthere.academy](/img/2022-01-14/dig_output.png)
 
But we’re not done yet. Having the domain and DNS, I had to bring GitHub up to date + configure HTTPS certificate. Luckily this one was really easy - go to repo’s Settings>>Pages and configure the custom domain + enforce HTTPS. This updates GitHub’s DNSes as well as propagates the certificate GitHub generates for you. The second operation may actually take a longer time (even a few hours).

![GitHub Custom Domain and HTTPS configuration](/img/2022-01-14/github_https_and_domain.png)
 
Still not fully there. What I still didn’t like was the fact my bare (apex) domain *itsthere.academy* served no content. Typing the address gave you "page not found" error. Maybe one day I will use it for a landing page of "It's There" project and take you to the blog, apps, other - will see how goes. For now it should take you to the blog. Could I use the CNAME solution here? 

No. For two reasons:
1. GitHub Pages certificate is valid for [blog.itsthere.academy](https://blog.itsthere.academy) domain only, so your browser would panic on untrusted [itsthere.academy](http://itsthere.academy) connection.
2. DNS specs won’t let you use CNAME record for apex (bare) domain. It has to be A record pointing at IP address. In simpler words - I could point my [itsthere.academy](http://itsthere.academy) domain at IP(s) only, but not at another domain. I will be honest - I wasn’t aware of this before :)

GitHub offers solution for apex domains - see [here](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-an-apex-domain). So I could solve #2, but not #1. Try generating a certificate to cover both domains? Well, I didn’t have to.

Not a surprise, I wasn’t the first person in the world to have this problem. In fact, the same issue is being solved when you type [itsthere-academy.github.io](https://itsthere-academy.github.io). All you need is a good, old HTTP redirect! Something to tell your browser - try again, but this time with [blog.itsthere.academy](https://blog.itsthere.academy). Since my DNS was on AWS, I used AWS solution for the redirect - nicely explained [here](https://aws.amazon.com/premiumsupport/knowledge-center/redirect-domain-route-53/). 

TL;DR:
1. create S3 bucket
2. set bucket's properties so it’s only purpose is redirecting to another location on the Web
3. create “A” (alias) record for the apex domain ([itsthere.academy](http://itsthere.academy)) to resolve to the bucket IP. Yes, since both DNS and S3 are AWS services, you can say “Alias for a bucket”.
4. celebrate

Simple to configure and inexpensive to run. Here’s what DNS says now:

![DNS output for itsthere.academy](/img/2022-01-14/dig_output_apex.png)
 
... and how the redirect works:

![HTTP redirect for itsthere.academy](/img/2022-01-14/curl_redirect.png)
 
Now all that was left was to setup local Jekyll instance, so I can test before I post. Find the default layouts are not exactly what I wanted. Try couple other from GitHub (and there’s plenty). Come back to a basic one, just don’t use the built-in copy as  outdated. Find there’s no space for logo, so dig into template’s and Jekyll manuals to find how to customize. Create the logo! And no, I’m no goddamn graphic designer!!! Dive into HTML and CSS to add the logo. Make sure the updated template still works on mobile. Realize you don't have a [favicon](https://pl.wikipedia.org/wiki/Favicon) and you want one. Then discover the favicon is more complicated than setting a property somewhere in HTML... but luckily there's plenty of generators online that will tell you what to copy-paste. Somewhere in the middle realize you now have two GitHub accounts (personal and blog), so you need your git to distinguish the two (btw: [really nice post on how to configure different SSH keys for different user and the same host - e.g. GitHub](https://www.section.io/engineering-education/using-multiple-ssh-keys-for-multiple-github-accounts/)). Realize you have no idea on “advertising” the blog, but the world is probably using Twitter? Should I create Facebook... maybe not, as we define “privacy” differently.

Create your first post.

**As you can see, setting up a blog is trivial and anyone can do it ;)** Maybe Medium or WordPress wasn't that bad 
idea, after all?

Naaaah. Maybe one day. I had to revisit so many things I otherwise almost forgot. That’s another reasons for this blog. Once all is up and running now, I really enjoy the overall experience. I can blog from VSCode and post with git. So geeky.

See you next time!
