---
excerpt: Creating a hackers blog - Using Jekyll
published: true
categories: [dev]
---

This post is part of a series

  1. [A hackers blog]({% post_url 2015-01-02-a-hackers-blog %})
  2. [The Jekyll framework]({% post_url 2015-01-11-the-jekyll-framework %})
  3. Blog automation

# The Jekyll framework
***

[Jekyll](http://jekyllrb.com/) is a framework that generates static html from simple text. It can utilize [Markdown](http://daringfireball.net/projects/markdown/) to generate html pages. It is the framework used by [GitHub Pages](https://pages.github.com/) which this site uses.

As with most Ruby based frameworks it has a Micro framework (mini web server) to allow local blog/site development without publishing to a live/test site.

The Jekyll source for this site is available on GitHub [https://github.com/devspecops/dumbsysadmin](https://github.com/devspecops/dumbsysadmin) should you need to reference components.

## Pre Requisites

- Installed version of Ruby 1.9.3
- Basic knowledge of HTML
- Basic knowledge of Javascript
- Basic knowledge of CSS

## Step 1 - Initialize Jekyll

First install Jekyll

```bash
gem install jekyll
```

We are also publishing to GitHub so to match GitHub requirements you will need to install the recommend gem.

```bash
gem install github-pages
```

Navigate to your GitHub site as you created in [A Hackers Blog]({% post_url 2015-01-02-a-hackers-blog %})

```bash
cd username.github.io
PROJECT=`pwd`
```

Initialize Jekyll Site

```bash
jekyll new .
```

This will create a vanilla Jekyll site. Should you decide the vanilla website is enough to get started on a blog site however this document is focused on creating a themed blog site.

Run micro framework

```bash
jekyll serv
```

Navigate to your vanilla website

```
http://127.0.0.1:4000/
```

## Step 2 - Install components

### Component parts

This site utilizes the following components

- GitHub Theme
- Bootstrap Framework
- Highlight.js

#### Github theme

This is one of the default themes available through GitHub pages. When creating a new site through [pages.github.com](http://pages.github.com), a wizard provides various themes to get started. One of themes is [Minimal](https://github.com/orderedlist/minimal).

The Minimal theme html tags...

~~~ html
<link rel="stylesheet" href="/stylesheets/styles.css">
<link rel="stylesheet" href="/stylesheets/pygment_trac.css">
~~~

#### Bootstrap framework

[Bootstrap](http://getbootstrap.com/) is a popular html framework to create attractive menus and layouts. It was initially created by Twitter.

This tutorial won't make you a bootstrap expert but it will provide a nice navigation bar at the top of the page.

Bootstrap html tags...

```html
<link rel="stylesheet" href="/stylesheets/bootstrap.min.css">
<script src="/javascripts/bootstrap.min.js"></script>
```

#### Syntax highlighting using highlight.js

[Highlight.js](https://highlightjs.org/) is a syntax highlighting framework with support for over 100 languages.

This site contains the defaults which Syntax Highlights popular programming languages. If your chosen language(s) are not available Highlight.js provides a [generator](https://highlightjs.org/download/) to support additional languages.

Highlight.js html tags...

```html
<script src="/javascripts/highlight.min.js"></script>
<script>hljs.initHighlightingOnLoad();</script>
```

#### Putting it all together

Create the following directories in project root.

```bash
PROJECT=`pwd`
mkdir ./{javascripts,stylesheets}
```

You can fetch the bootstrap.min.css, bootstrap.min.js, highlight.min.css and highlight.min.js directly from the vendor site or just download the ones used in this site from GitHub.

```bash
## Stylesheets
cd $PROJECT/stylesheets

# highlight.js
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/stylesheets/highlight.min.css

# Bootstrap
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/stylesheets/bootstrap.min.css

# GitHub theme
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/stylesheets/styles.css
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/stylesheets/pygment_trac.css

## Javascripts
cd $PROJECT/javascripts

# highlight.js
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/javascripts/highlight.min.js

# bootstrap
wget https://raw.githubusercontent.com/devspecops/dumbsysadmin/gh-pages/javascripts/bootstrap.min.js
```

Now that the Stylesheets (CSS) and Javascripts have been downloaded its time to create the default layout.

### Step 3 - Default layout page

Jekyll makes uses layout pages. This has a predefined directory to store layouts

```
$PROJECT/_layouts
```

Create the about page which is also the index.html page.

```bash
touch $PROJECT/_layouts/default.html
```

#### YAML Front matter

Jekyll dynamically renders any document/page that starts off with YAML [Front matter](http://jekyllrb.com/docs/frontmatter/). This is YAML data between a set of starting and ending triple-dashed lines.

*Example front matter preventing a page from being published*

```yaml
---
publish: false
---
```

We will start off with adding blank front matter to prompt Jekyll to dynamically render the default layout page.

```bash
cat <<EOF > $PROJECT/_layouts/default.html
---
---
EOF
```

Editing the default layout page with your favourite editor. I use Vim (be it a [vimplugin](https://github.com/JetBrains/ideavim) with my favourite [IDE](https://www.jetbrains.com/ruby/)) to do all my editing.

```bash
vim $PROJECT/_layouts/default.html
```

Populate the page with the following html, leaving the YAML front matter in place.

```html
---
---
<!doctype html>
<html>
<head>
    <title>Place your title here</title>

    <!-- Bootstrap theme -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/bootstrap.min.css">

    <!-- Default theme from GitHub -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/styles.css">
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/pygment_trac.css">

    <!-- Highlight theme -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/highlight.min.css">

    <!-- Bootstrap javascript -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/bootstrap.min.js"></script>

    <!-- Highlighting javascript -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/highlight.min.js"></script>
    <script>hljs.initHighlightingOnLoad();</script>

    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <!--[if lt IE 9]>
    <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->

    <!-- Work with mobiles? -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/scale.fix.js"></script>
</head>
<body>
{% raw %}{{content}}{% endraw %}
</body>
</html>
```

#### Liquid Markup

Observant readers will notice that there is a non HTML element within the default layout page.

```
{% raw %}{{content}}{% endraw %}
```

This is part of the [Liquid markup](http://liquidmarkup.org/) that Jekyll uses to generate html pages.

The {% raw %}{{ content }}{% endraw %} directive places text content directly into the layout page.

We are now going to create the content that is placed in the {% raw %}{{ content }}{% endraw %} section.

### Step 4 - Create the index/about page

Create the *index.html* page which will also become our site page.

```bash
cat <<EOF > $PROJECT/index.html
---
layout: default
---
EOF
```

Notice that we are specifying the layout default in the YAML front matter. The page *index.html* will be wrapped around the layout default which was freshly created.

Edit the page and add the rest of the content. The components will be explained shortly.

```html
---
layout: default
---
<div class="wrapper">
    <!-- Navigation bar -->
    <nav class="navbar navbar-default">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">blog:</a>
        </div>
        <div class="container-fluid">
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li><a href="{% raw %}{{ site.baseurl }}{% endraw %}/posts.html">Posts</a></li>
                </ul>
                <ul class="nav navbar-nav navbar-right">
                    <li class="active"><a href="{% raw %}{{ site.baseurl }}{% endraw %}/index.html">About</a></li>
                </ul>
            </div>
        </div>
    </nav>

    <!-- Page header -->
    <header>
        <h1>The Awesome Blog</h1>

        <p>Come here for new and interesting content</p>

    </header>

    <!-- Page section -->
    <section>
        <h3>
            <a id="about" class="anchor" href="#about" aria-hidden="true">
                <span class="octicon octicon-link"></span></a>About
        </h3>

        <p>Write something interesting about your experience here.</p>

        <p>Write something about what you like to do here.</p>

        <h3>
            <a id="about" class="anchor" href="#why" aria-hidden="true">
                <span class="octicon octicon-link"></span></a>Contact
        </h3>

        <p>Place some contact information here</p>
    </section>

    <!-- Page footer -->
    <footer>
        <p>
            <small>Hosted on GitHub Pages</small>
        </p>
    </footer>
</div>
```

#### Page components

##### Bootstrap Navbar

```html
<!-- Navigation bar -->
<nav class="navbar navbar-default">
  ...
</nav>
```

This creates the navigation bar at the top of the page more information can be found from [documentation](http://getbootstrap.com/components/#navbar) page

##### Header / Section / Footer
```html
<!-- Page header -->
<header>
  ...
</header>

<!-- Page section -->
<section>
  ...
</section>

<!-- Page footer -->
<footer>
  ...
</footer>
```

This is part of the [Minimal](https://github.com/orderedlist/minimal) theme and is responsible for re organising the layout when displaying on small/large screens.

Next create the posts page

## Step 5 - Create posts page

Create the *posts.html* page which will become the landing paste for all posts.

```bash
cat <<EOF > $PROJECT/posts.html
---
layout: default
---
EOF
```

The posts page

```html
---
layout: default
---
<div class="wrapper">
  <nav class="navbar navbar-default">
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
          aria-expanded="false" aria-controls="navbar">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">blog:</a>
    </div>
    <div class="container-fluid">
      <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
        <ul class="nav navbar-nav">
          <li class="active"><a href="{% raw %}{{ site.baseurl }}{% endraw %}/posts.html">Posts<span class="sr-only">(current)</span></a></li>
        </ul>
        <ul class="nav navbar-nav navbar-right">
          <li><a href="{% raw %}{{ site.baseurl }}{% endraw %}/index.html">About</a></li>
        </ul>
      </div>
    </div>
  </nav>
  <header>
    <h1>Blog posts</h1>

    <p>Interesting tech posts</p>
  </header>
  <section>
    <h3>
      <a id="about" class="anchor" href="#about" aria-hidden="true">
        <span class="octicon octicon-link"></span></a>Posts
  </h3>
    {% raw %}{% for post in site.posts %}
      <p>
        <a href="{{ post.url }}">{{ post.date | date_to_long_string }} {{ post.title }}</a><br>
        {{ post.excerpt }}
      </p>
    {% endfor %}{% endraw %}
  </section>
  <footer>
    <p>
      <small>Hosted on GitHub Pages</small>
    </p>
  </footer>
</div>
```

## Step 6 - Posts Layout

Just as a layout was created for the *index.html* and *posts.html* page we are going to create one as well for individual posts.

```html
---
---
<!doctype html>
<html>
<head>
    <title>Place your title here</title>

    <!-- Bootstrap theme -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/bootstrap.min.css">

    <!-- Default theme from GitHub -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/styles.css">
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/pygment_trac.css">

    <!-- Highlight theme -->
    <link rel="stylesheet" href="{% raw %}{{ site.baseurl }}{% endraw %}/stylesheets/highlight.min.css">

    <!-- Bootstrap javascript -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/bootstrap.min.js"></script>

    <!-- Highlighting javascript -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/highlight.min.js"></script>
    <script>hljs.initHighlightingOnLoad();</script>

    <meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no">
    <!--[if lt IE 9]>
    <script src="//html5shiv.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->

    <!-- Work with mobiles? -->
    <script src="{% raw %}{{ site.baseurl }}{% endraw %}/javascripts/scale.fix.js"></script>
</head>
<body>
<div class="wrapper">
    <nav class="navbar navbar-default">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#navbar"
                    aria-expanded="false" aria-controls="navbar">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="#">blog:</a>
        </div>
        <div class="container-fluid">
            <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                <ul class="nav navbar-nav">
                    <li class="active"><a href="{% raw %}{{ site.baseurl }}{% endraw %}/posts.html">Posts<span class="sr-only">(current)</span></a></li>
                </ul>
                <ul class="nav navbar-nav navbar-right">
                    <li><a href="{% raw %}{{ site.baseurl }}{% endraw %}/index.html">About</a></li>
                </ul>
            </div>
        </div>
    </nav>
    {% raw %}{{content}}{% endraw %}
</div>
</body>
</html>
```

## Step 7 - Your first post

Jekyll renders posts that are stored under the *$PROJECT/_posts* directory. Posts can be Markdown, Textile or HTML.

First lets create a markdown page *$PROJECT/_posts/YYYY-MM-DD-TITLE.md*.

```bash
touch $PROJECT/_posts/`date "+%Y-%m-%d"`-your-first-post.md
```

Populate your post with

```markdown
---
excerpt: Excerpt for your very first post
published: true
---
# Your first post
---

## Some really interesting post

Some content for your post
```

Your Blog site is ready to roll.
