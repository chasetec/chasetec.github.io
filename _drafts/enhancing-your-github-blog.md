---
layout: post
title:  "Enhancing your GitHub Pages Blog"
categories: jekyll update
---

[Previously][starting-a-blog] I talked about how to setup a GitHub Pages blog using Jeykll. After getting your blog functional there are still a lot of things you can do to enhance it which I'll be talking about here.

## Setting up a domain name

I want to use <http://blog.osdev.org> instead of <http://chasetec.github.io>.

Create a `CNAME` file in your Pages repo with your domain or subdomain name. Mine will contain:

    blog.osdev.org

For a subdomain (what I'll be doing) edit your DNS records to contain a CNAME record that points from your subdomain to your Pages host name.

    blog.osdev.org  1800  IN  CNAME  chasetec.github.io

For domains that are not subdomains you'd instead create an A record pointing to `204.232.175.78`. See the [official instructions][pages-domain] more information. 

[starting-a-blog]: http://chasetec.github.io/github/jekyll/2013/12/11/starting-a-github-blog.html
[pages-domain]: https://help.github.com/articles/setting-up-a-custom-domain-with-pages
