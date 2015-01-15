---
excerpt: Creating a hackers blog - The making of this site
published: true
categories: [dev]
---

This post is part of a series

  1. [A hackers blog]({% post_url 2015-01-02-a-hackers-blog %})
  2. [The Jekyll framework]({% post_url 2015-01-11-the-jekyll-framework %})
  3. Blog automation

# A hackers blog
***

It takes a lot of effort to start off anything new, this holds true for software projects.
Consider for a second all the parts that make up a well written software project...

  * source control
  * unit tests
  * documentation
  * Makefile (Rakefile) or equivalent
  * package dependencies
  * layouts for web development
  * the list goes on...

This is what I've done to make this site

The choice for hosting providers will dictate what technological choices you can make. These days I like coding in Ruby,
and using Git for source control.

[Github Pages](https://pages.github.com/)

[Sonia Hamilton](http://blog.snowfrog.net/) suggested using Github Pages which ticked all the right boxes for me.

### Step 1 Create an account

If you don't already have a Github account you will need to create one...

[https://github.com/](https://github.com/)

### Step 2 Create a new repository

Create a new GitHub repository named username.github.io, where username
is your Github account username. You can also create an
[organization](https://help.github.com/articles/creating-a-new-organization-from-scratch/)
and use organisation.github.io.

<img src='/images/creategithubpage.png' alt='Creating a GitHub page'>

### Step 3 Clone repository

~~~ bash
git clone https://github.com/username/username.github.io
~~~

GitHub uses the branch *gh-pages* to publish content. Ensure that the branch is available.

~~~ bash
# Check if gh-pages branch exists
git branch | grep 'gh-pages'
# Create if gh-pages is missing
git branch -b gh-pages
~~~

### Step 4 Create simple html page

~~~ html
<!-- username.github.io/index.html -->
<html>
<body>
Hello World<br>
</body>
</html>
~~~

### Step 5 Push

~~~ bash
git add --all
git commit 'Initial commit'
git push
~~~

***

Next in this series Jekyll Framework

