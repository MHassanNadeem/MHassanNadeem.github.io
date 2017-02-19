---
layout: post
title: How to set custom height and width of embedded Github Gist
description: Learn how to add CSS to customize the dimentions of embedded github gists so that they don't overshadow rest of your blog.
tags: ['github', 'gist', 'css']
---
<style>
.mini-gist-cssreset .gist {
   max-width:350px;
   overflow:auto;
}

.mini-gist-cssreset .gist .blob-wrapper.data {
   max-height:200px;
   overflow:auto;
}
</style>

<div class="yellow-box"><strong>Note: </strong>This post was written when I was hosting my blog on blogger. I have since moved to jekyll powered blog hosted on github-pages.</div>

I am in love with Github Gists, they provide an quick and easy way to share code snippets with highlighted syntax. I use embedded gists to add beautifully highlighted code to my blog. But embedding large code with default styling can potentially overshadow the actual content of the blog. I had a similar problem today when I had to share a relatively lengthy snippet of code. This prompted me to look into the CSS of the gist and come up with custom styling to specify its width and height. After studying the styling I came with the following CSS to adjust the maximum dimensions of gist:

{% highlight css %}
.gist {
   max-width:350px;
   overflow:auto;
}

.gist .blob-wrapper.data {
   max-height:200px;
   overflow:auto;
}
{% endhighlight %}

If you are using blogger to host your blog (like I am), you can add the CSS under Template → Customize → Advanced → Add CSS, so that the CSS applies to all of your blog posts. Or else you can add the CSS inline within the <script> </script> tags to apply the style to specific gists only.

As an example consider the following Github Gist of original Gist Stylesheet with height of 200px and width of 350px.
<div class="mini-gist-cssreset" align="center">
  <script src="https://gist.github.com/MHassanNadeem/412662ca64e0752983ba.js"></script>
</div>
