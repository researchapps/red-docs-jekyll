---
sectionid: set-up
sectionclass: h1
is-parent: yes
title: Set Up
number: 2000
---

The general idea is that you are going to be able to host this on <a href="https://help.github.com/articles/what-is-github-pages/" target="_blank">github-pages</a>, which is available to anyone with a Github account. We have provided a Dockerized development environment, unless you want to <a href="https://jekyllrb.com/docs/installation/">install Jekyll yourself</a>.

When you are ready to go, you can download the theme, and then start developing your site. Just navigate to the folder, open a terminal and type:

{% highlight bash %}
jekyll serve
{% endhighlight %}

You will then be able to see the page at **localhost:4000** and make your changes. When you are happy just push them to github in a gh-pages branch, or the master branch of your organization or user, and you're site will be compiled by GitHub for you.
