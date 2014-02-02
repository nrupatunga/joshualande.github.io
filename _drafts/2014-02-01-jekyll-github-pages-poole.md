---
layout: page
title: How I Created a Beautiful and Minimal Blog Using Jekyll, Github Pages, and poole
comments: true 
---

I recently migrated this website from [Wordpress](http://wordpress.com) to [Github pages](http://pages.github.com/)
using [Jekyll](http://jekyllrb.com) and [poole](https://github.com/poole/poole). So far I am really happy with the transition. Over time, I grew to really dislike how heavy-weight Wordpress is. I dislike how WYSIWYG editors make it really hard to see what HTML is being generated and tend to totally bloat the code that is produced. And I found the plugin system so confusing that I was always afraid to try to customize the layout of anything.

The process of backing up Wordpress is a huge pain. I was constantly annoyed by having to upgrade Wordpress to avoid security risks. I hated having to keep an opaque database in order to keep all of my content. And whenever I went in to edit my website, I was always afraid that I would fat-finger change something and have no idea what happened. 


So when I learned about [Jekyll](http://jekyllrb.com/), it seemed like a great alternative. I like the idea that my entire blog is a set of static files. Besides its simplicity, it makes backups so much easier and avoids most common security concerns caused by running dynamic websites. I could write my posts in [Markdown](http://en.wikipedia.org/wiki/Markdown) which I am used to from [Confluence](https://www.atlassian.com/software/confluence) and [GitHub](http://github.com). And  I was really excited that all of my blog posts would be static markdown files and I wouldn't have to deal with configuring a website. Also, Jekyll allows for code examples to be very nicely embedded in the website. Finally, Jekyll being so lightweight allowed me to make my website be super-minimal and remove most of the bloat common to other websites.

The fact that GitHub provides [free hosting for Jekyll blogs](http://pages.github.com) is just icing on the cake. It will save me ~$50 per year in hosting. GitHub provides automatic version control of my blog. I can use GitHub's web editor to write blog posts online. And I can still connect it to my custom domain.

# Getting Started

Instead of reading a lot of documentation, I found this really great git repo called [poole](https://github.com/poole/poole). poole provides:

> "a clear and concise foundational setup for any Jekyll site. And it has a super minimal look which I was interested in. It does so by furnishing a full vanilla Jekyll install with example templates, pages, posts, and styles."

[Here](http://demo.getpoole.com/) is a demo of the poole website. It looks like

![The demo pool website](/assets/demo_poole_website.png)

To get started with my blog, all I had to do was create a new Git repo with the name [joshualande.github.io](joshualande.github.io), download the poole repository, and push it to my git repo. A few minutes later the website joshualande.github.io was be ready! I only had a few posts on my previous website so I josh copied them over manually. But there is [a package](http://jekyllrb.com/docs/migrations) for migrating blogs to Jekyll.

# Blog Layout

The initial state of the [poole](https://github.com/poole/poole) repository is:

{% highlight sh %}
$ ls -1
404.html
CNAME
LICENSE.md
README.md
_config.yml
_includes
_layouts
_posts
about.md
atom.xml
index.html
public
{% endhighlight %}

You can view the folder structure on [GitHub](https://github.com/poole/poole).
When you run Jekyll, it creates a folder called _site with the
static website. Every file or folder in the repo will get copied 
into the website unless it begins with an underscore.
Markdown files will get automatically converted to HTML.
And poole uses the [Liquid](http://liquidmarkup.org) templating system to allow
for somewhat dynamic content on the website.

The folder [_posts](https://github.com/poole/poole/tree/master/_posts) contains all of the blog posts in markdown format.
The example posts that come with poole are:
{% highlight html %}
$ ls -1 _posts/
2013-12-31-whats-jekyll.md
2014-01-01-example-content.md
2014-01-02-introducing-poole.md
{% endhighlight %}
[index.html](https://github.com/poole/poole/blob/master/index.html) 
contains the front page of the blog and 
[about.md](https://github.com/poole/poole/blob/master/about.md) is a
static post in markdown format.
If you want to have more static files, you can just add them to the
repo and poole will copy them to the _site folder when rendering the website.

[_config](https://github.com/poole/poole/blob/master/_config.yml) 
contains general configuration stuff for the website.
{% highlight yaml %}
# Setup
title:            Poole
tagline:          'The Jekyll Butler'
description:      'Base theme for Jekyll themes by @mdo.'
url:              http://getpoole.com
...
{% endhighlight %}

Finally, the [_layouts](https://github.com/poole/poole/blob/master/_layouts) 
and [_includes](https://github.com/poole/poole/tree/master/_includes)
folders contain boiler-plate HTML for building the website.
{% highlight yaml %}
$ ls -1 _layouts/
default.html
page.html
post.html
$ ls -1 _includes/
head.html
{% endhighlight %}
It is not very hard to go in and modify those files to change the look and feel of the site.

# Customizations 

To get my blog to its current form, I made a few modifications to the base poole layout. 

FIrst, I wanted to create an [Archive](/archive) page which listed all of my blog posts.
To do this, I created the file [archive.md](https://github.com/joshualande/joshualande.github.io/blob/5e5ca6389fbc66be06488b9b7803e0278ee1b89f/archive.md) which shows a dynamic list of all of 
my blog posts:
{% highlight html %}
{% raw %}
---
layout: page
title: Archive
---

# Blog Posts

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
{% endraw %}
{% endhighlight %}

Next, I wanted to add a navigation bar at the top of the website with links to the [About](/about) page, [Archive](/archive) page, and the feed. To do this, I modified the file [_config.yml](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_config.yml) to define a dictionary of pages to show in my header:
{% highlight yaml %}
pages_list: {'About':'/about','Archive':'/archive','Feed':'/atom.xml'}
{% endhighlight %}
I then modify the file [_layouts/default.html](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_layouts/default.html) to loop over this list, creating links to each of the pages:
{% highlight html %}
{% raw %}
<h3 class="masthead-title">
<a href="/" title="Home">{{ site.title }}</a>

{% for page in site.pages_list %}
  &nbsp;&nbsp;&nbsp;
  <small><a href="{{ page[1]  }}">{{ page[0] }}</a></small>
{% endfor %}
</h3>
{% endraw %}
{% endhighlight %}


# Disqus Comments

I wanted to enable [Disqus](http://disqus.com/) comments on the
blog. To do that, I modified the file [_layouts/default.html](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_layouts/default.html) to include the line:
{% highlight python %}
{% raw %}
{% include comments.html %}
{% endraw %}
{% endhighlight %}

I then created a file [_includes/comments.html](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_includes/comments.html) which includes the code given to my by Disqus:
{% highlight html %}
{% if page.comments %}
<!-- Add Disqus comments. -->
<div id="disqus_thread"></div>
<script type="text/javascript">
  /* * * CONFIGURATION VARIABLES: EDIT BEFORE PASTING INTO YOUR WEBPAGE * * */
  var disqus_shortname = '<USERNAME>'; // required: replace example with your forum shortname

  /* * * DON'T EDIT BELOW THIS LINE * * */
  (function() {
    var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
    dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
  })();
</script>
<noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
<a href="http://disqus.com" class="dsq-brlink">comments powered by <span class="logo-disqus">Disqus</span></a>
{% endif %}
{% endhighlight %}
By setting up the code this way, I can enabled commenting on a page-by-page basis. All I have to do is set "comments: True" in the YAML header of the post.

# Google Analytics

Finally, I enabled [Google Analytics](http://www.google.com/analytics) on the website. First, I had to register my website through
the Google Analytics. Google gave me some tracking code to embed on every website which I put in the file
[\_includes/google\_analytics.html](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_includes/google_analytics):
{% highlight html %}
<script>
  (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
  (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
  m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
  })(window,document,'script','//www.google-analytics.com/analytics.js','ga');

  ga('create', 'UA-47674613-1', 'joshualande.com');
  ga('send', 'pageview');

</script>
{% endhighlight %}
Finally, I included this tracking code on all pages of my website by modifying the file
[_layouts/default.html](https://github.com/joshualande/joshualande.github.io/blob/64d03b883b64dd8aedf30b903ecaae92a282955a/_layouts/default.html)
to include the line:
{% highlight html %}
{% raw %}
{% include google_analytics.html %}
{% endraw %}
{% endhighlight %}

# Getting a Custom URL

Once I got my blog up to speed on GitHub with the URL joshualande.github.io, it was easy to link my personal domain [joshualande.com](http://joshualande.com) to it. I use Namecheap to host my domain, so I followed the instructions [here](http://davidensinger.com/2013/03/setting-the-dns-for-github-pages-on-namecheap).

If you have any questions about my implementation, you can view my website on [GitHub](https://github.com/joshualande/joshualande.github.io) or leave a question on this post.

# Links:

Here are some links which helped me along the way:

* The official [Jekyll](http://jekyllrb.com) website, along with detailed [documentation](http://jekyllrb.com/docs/home).
* Documentation by GitHub about hosting static webpages with [GitHub pages](http://pages.github.com).
* The [poole](https://github.com/poole/poole) GitHub repository.
* The GitHub repository for my [personal website](http://github.com/joshualande/joshualande.github.io).