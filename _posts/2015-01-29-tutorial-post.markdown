---
layout: post
title:  "Tutorial Blog Post in Jekyll"
date:   2015-01-27 13:41:00
category: web
author: firoze
---

## Writing a Blog Post in Jekyll

So you've got some bench time and want to contribute to the #team-stanton blog. If you're already familiar with [Markdown](http://en.wikipedia.org/wiki/Markdown), you're halfway there. This post will only talk about some stuff you have to make sure to do when putting together your awesome new post for the blog, as well as some features that make your life easier, so you can focus more on getting your thoughts and information across.

# Naming/Placing Your Posts

All posts will go under the `_posts` folder. The file naming convention in Jekyll is as follows:  
`YYYY-MM-DD-title.MARKUP`

- `YYYY` - the current year
- `MM` - the current month
- `DD` - the current day (zero-padded if single digit)
- `title` - short title of your post.
  - if you want to put multiple words, you can separate them by using `-`
- `MARKUP` - Jekyll supports two markup languages by default.
  - [Markdown](http://daringfireball.net/projects/markdown/) - extension `md` or `markdown`
  - [Textile](http://redcloth.org/textile) - extension `textile`

The file for this post, for example, is named `2015-01-29-tutorial-post.markdown`.

When you are testing locally, `bundle exec jekyll serve` will automatically find all posts (including your new one), and generate the site and host it at `http://localhost:4000/teamstanton`.

# Mandatory Post Metadata

In the beginning of the post file, the following block has to be present, with fields specific to you filled in, to allow Jekyll to interpret them and display them in the blog index page (as well as the post page). The data has to be entered in the form of a YAML set (Jekyll calls this "[front matter](http://jekyllrb.com/docs/frontmatter/)") and, as mentioned, should be the first thing in the post file.

{% highlight yaml %}
---
layout: post
title:  "Your Title Here"
date:   YYYY-MM-DD HH:MM:SS
category: <category>
author: <author>
---
{% endhighlight %}

The author, for your convenience, can be your Github username. The category is (for now) a single word (perhaps the platform that your post belongs to). In the future, we will try to incorporate keywords into the blog.

# Syntax Highlighting

Jekyll uses [Pygments](http://pygments.org/) as its built-in "highlighter", and it (along with any dependencies) are automatically installed when you install Jekyll, either on its own, or via `bundle install` when setting your system up for Github Pages.

Consider the following example (taken from Jekyll docs):
![](http://i.imgur.com/pSO1jy5.png)

Replace `ruby` with the language of your choosing, and ender the code between the `highlight`/`endhighlight` tags. The following is a snippet in `Java`:

{% highlight java %}
class Ideone {
  public static void main (String[] args) throws java.lang.Exception {
    System.out.println("Hello World!");
  }
}
{% endhighlight %}

You can also open up any of the posts (including this one) in your favorite text editor and look at the raw markdown behind it.

# Did you want to style something?

Jekyll's default Markdown interpreter/converter is [kramdown](http://kramdown.gettalong.org/) and one of its features is that we can set [CSS](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) styles (defined elsewhere). Since everything in markdown is basically a series `<p>` tags, one (or more) classes will get applied to the corresponding tag, and you can fine-tune your CSS by using selectors.

For example, let's say you have a device screenshot which has a resolution of 1920x1080 and you don't necessarily want to resize it and maintain it alongside the original size. To set a CSS class, all you have to do is:

```{:.className}```

...and the next immediate `<p>` tag will have that class. So, if you wanted to add a class "reducedScreenshot" to an image, your markdown document would look like:

``{:.reducedScreenshot}``  
``![](http://link.to.your/screenshot.png)``

Alternately, you can add the class directly on the image like so:

``![](http://link.to.your/screenshot.png){:.reducedScreenshot}``

Jekyll also uses [SASS](http://sass-lang.com/), so the styling process is slightly friendlier :)

Now, to define your class, go into the `_sass` and add it to an existing file (like the `_posts.scss` file), or create a new `.scss` file of your choosing (make sure it is prepended by `_` as Jekyll uses that as an identifier to interpret files).

If you created your own file, make sure to add it to the list of files to include in the main stylesheet by heading over to `css/main.scss`, scrolling to the bottom, and adding it to the list of imports. (you could also just define your class in the `main.scss` file, but it helps with modularity if you keep it as a separate file).

To take a look at an example, check out the `2015-01-27-firoze-notifications-android.markdown` file in a text editor, and use a web inspector to look at the corresponding screenshot in the interpreted post.

# Get Blogging!

If you have any questions, please do not hesitate to contact anyone else in your team, or to look at the documentation relevant to your question.

Thank you for reading, and happy blogging!
