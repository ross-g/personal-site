---
title: Theme update
tags:
  - Blog
---

Well I've managed to spend a few weekends now getting to a point where I'm happy with the blogs look and how I am able work on content within it. Enabling per-post comments by integrating [Disqus](https://disqus.com/) with the site wasn't much of a chore at all. Mostly because Disqus make this ridiculously easy; copy and paste a html snippet and set a few parameters.
So that was a quick win, see below.

{% highlight html %}
<div class="comments">
	<div id="disqus_thread"></div>
	    <script type="text/javascript">
	        var disqus_shortname = 'example'; // replace example with your forum shortname

	        (function() {
	            var dsq = document.createElement('script'); dsq.type = 'text/javascript'; dsq.async = true;
	            dsq.src = '//' + disqus_shortname + '.disqus.com/embed.js';
	            (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(dsq);
	        })();
	    </script>
	    <noscript>Please enable JavaScript to view the <a href="http://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
</div>
{% endhighlight %}

I've also managed to apply and tweak a decent CSS theme to make the site as a whole more readable. The theme I'm using is heavily based on [Midnight](http://jekyllthemes.org/themes/midnight/) by [Matt Graham ](http://madebygraham.com/) and gave be basically exactly what I wanted with a few minor tweaks. This took a little more time than expected! But if you're trying to adopt only certain parts of code from one environment and wanting them to work properly inside a different one ... then I should have expected some iteration.

Remaining work?
As far as the site goes I'd still like to add a tagging system, but that could quite easily be implemented retroactively once it's needed. For now I'd like to get my head back into some Python and Maya tools work.
