---
title: Multiple Authors in Jekyll
author: juliano
---

Hi!
This post is about how to add author's information at the end of each post in Jekyll powered blogs.
I followed two good references found in the web: [SorryApp](http://blog.sorryapp.com/blogging-with-jekyll/2014/02/06/adding-authors-to-your-jekyll-site.html) and [lostdecadagames](http://www.lostdecadegames.com/blog-author-attribution-using-jekyll/).

First of all, we need to store the author information somewhere. I created a dir named ***_data*** with a file named ***authors.yml***
Following is the content of authors.yml:

{% highlight yaml linenos %}
juliano:
    name: Juliano Fischer Naves
    email: julianofischer@gmail.com
    web: www.julianofischernaves.me
    gravatar: 6c9edb3c5236bbd4535fd69a955fcee7
{% endhighlight %}

Then I modified the ***_layouts/post.html***

{% highlight html linenos %}

{% raw %}
{% assign author = site.data.authors[page.author] %}
{% if author %}

    <div style="width: 20%; float:left">
        <img src="http://www.gravatar.com/avatar/{{ author.gravatar }}?s=40">
    </div>

    <div style="width: 80%; float:left">
        Written by <a href="{{ author.web }}" target="_blank">{{ author.name }}</a> in {{ page.date | date_to_string }}.
    </div>
{% endif %}
{% endraw %}
{% endhighlight %}


Finally, we need to include at the beginning of each post the author's identification:

{% highlight yaml linenos %}
---
title: Your Post Title
author: juliano
---
{% endhighlight %}

Well, the next step is to solve how to insert subjects for each post.
I hope to write about it soon.

Thank you for your attention!
