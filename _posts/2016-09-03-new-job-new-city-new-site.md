---
layout: post
title: New Job, New City, New Site
---

This past month, I took a job as a backend engineer at [Edlio](https://www.edlio.com/) and moved to Los Angeles, all the way from Brooklyn. Whew.

Needless to say, this brought a lot of change into my life. And while it's been a little rough adjusting, it's been a good change overall! 

I figured that if I was going to go through all these changes, so should my personal site. It used to be a simple résumé page with some (ugly) pictures, which wasn't too exciting. 

When thinking of what I should replace it with, I thought of Jonathan Blow's [blog](http://number-none.com/blow/blog/). But unlike him, I don't consider myself an authority on anything nor do I have as much to really say (which is why I find Twitter great). So... _should_ I?

Eh, why not? I mean look, I have nice code snippets with ligature support:
<br>
{% highlight kotlin %}
// This is a simple translation of an example from
// the Jai "quick lambda" compiler demo

// This actually wouldn't compile as the compiler needs 
// explict type information
fun main(args: Array<String>) {
    doIt(10, ::squarePoly)
}

fun <T> doIt(x: T, func: (T) -> T) {
    print("Result is ${func(x)}")
}

fun <T> squarePoly(x: T): T {
    return x * x
}
{% endhighlight %}
<br>
Pretty nice right? In all seriousness, I expect to do a few write-ups as I grow and learn as an engineer, so this feature is pretty important. I wanted to be able to communicate my ideas clearly and effectively as they are demonstrated through not only prose, but code as well.

That being said, I don't expect this to just be a programming blog. I want to write about things that interest me, even if they might seem mundane to others. Trying to *communicate* effectively is a big part of why I'm doing this. I often find it hard for me to thoroughly explain things and forcing myself to practice doing so is the first step to fixing that. Chris Poole, who is one of my biggest inspirations, wrote an excellent [post](http://chrishateswriting.com/post/91255458913/writing) on this very thing!

As weird as it sounds, I feel as though writing is an important part of being human. Literacy is part of what separates us from the rest of the animal kingdom, isn't it?