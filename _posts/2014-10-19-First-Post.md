---
title: Hello Internet
tags:
  - Blog
---

Unfortunately I'm suffering something of a drought at work lately in terms of technical challenge. So I'll be indulging myself in my spare time more than usual.
This weekends results of which are now online here.

Hosting and publishing a simple website through a GitHub repository is actually a pretty elegant idea. No complex CMS system or databases to run, *literally* nothing to install or run on your local machine if you don't want to. (yay for zero dependancies!) Of course this has some inherent limitations... but for a simple site with mostly static content it's efficient.

[GitHub Pages](https://pages.github.com/) offer a pretty straightforward way to get started and you can use some of their empty site templates, or you can simply clone an existing template repository that has some features already implemented. For instance GitHub run all their pages through [Jekyll](http://jekyllrb.com/) when publishing.Jekyll is a static site generator, you supply it markdown, HTML & CSS files and a website full of content comes out the other end... or that's the theory. To get a head start (and avoid some head aches) I forked the [Jekyll Now](https://github.com/barryclark/jekyll-now) repository to get a good basic site structure I could edit quickly.

To add content it's a simple matter of creating a markdown file inside the `_posts` folder, named `YYYY-MM-DD-Page-Title.md` and filling it with content. Remembering to include the *front-matter* markdown at the top of every post.

Quite a lot of markdown is available ([Markdown basics](https://help.github.com/articles/markdown-basics/), [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet), [Emoji](http://www.emoji-cheat-sheet.com/)) and including, happily for me, Python [syntax highlighting](http://pygments.org/docs/lexers/)! :smile:
{% highlight python %}
@requires_authorization
def somefunc(param1='', param2=0):
    r'''A docstring'''
    if param1 > param2: # interesting
        print 'Gre\'ater'
    return (param2 - param1 + 1 + 0b10l) or None

class SomeClass:
    pass

>>> message = '''interpreter
... prompt'''
{% endhighlight %}

There's still some tinkering (read: fixing and head-scratching) to be done for improvements sake. Mainly I'd like to include a blog tag-index and per-post comments. But for now it's off to a good start.
