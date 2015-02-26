---
layout: post
title:  "Welcome to Jekyll!"
date:   2015-01-26 09:45:03
category: web
author: gotoplanb
---
You’ll find this post in your `_posts` directory. Go ahead and create your own and re-build the site to see your changes. You can rebuild the site in many different ways, but the most common way is to run `bundle exec jekyll serve` or `make` (because of the `Makefile`), which launches a web server and auto-regenerates your site when a file is updated. View this file in your favorite text editor to see the other properties you have to set for this post (such as layout, author, category etc.) to make it your own.

To add new posts, simply add a file in the `_posts` directory that follows the convention `YYYY-MM-DD-name-of-post.ext` and includes the necessary front matter. Take a look at the source for this post to get an idea about how it works.

Jekyll uses `kramdown` as its Markdown parser. You can refer to their [documentation](http://kramdown.gettalong.org/) for more advanced features.

Jekyll also offers powerful support for code snippets:

{% highlight ruby %}
def print_hi(name)
  puts "Hi, #{name}"
end
print_hi('Tom')
#=> prints 'Hi, Tom' to STDOUT.
{% endhighlight %}

Check out the [Jekyll docs][jekyll] for more info on how to get the most out of Jekyll. File all bugs/feature requests at [Jekyll’s GitHub repo][jekyll-gh]. If you have questions, you can ask them on [Jekyll’s dedicated Help repository][jekyll-help].

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help

You can embed videos like so:

{% highlight html %}
{::nomarkdown}
<div class="embed">
	<iframe src="https://www.youtube.com/embed/PNcDI_uBGUo" frameborder="0" allowfullscreen></iframe>
</div>
{:/nomarkdown}
<!-->wrap them in {nomarkdown} tags to let kramdown know that this portion is not to be parsed <-->
{% endhighlight %}

The result will be something like this:

{::nomarkdown}
<div class="embed">
	<iframe src="https://www.youtube.com/embed/PNcDI_uBGUo" frameborder="0" allowfullscreen></iframe>
</div>
{:/nomarkdown}

This uses a default 16x9 aspect ratio. If you need a 4x3, use `class="embed ratio-4x3"` instead. If you we need more ratios, add them to `_sass/modules/_embeds.scss`.

