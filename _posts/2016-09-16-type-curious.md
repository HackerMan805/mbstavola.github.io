---
layout: post
title: Type Curious
---

So, one of my first assignments at Edlio was helping implement our new [staff](http://www.edliohs.org/apps/staff/){:target="_blank"} [directory](http://www.edlioelementary.com/apps/staff/departmental.jsp){:target="_blank"} [feature](http://www.edlioms.org/apps/staff/){:target="_blank"}. It was super fun to work on and allowed me to really stretch my legs and explore a mostly unfamiliar codebase!

While the links above only show off the public side of things, a big part of this was providing a lot of customization options for our customers. Selecting which fields to display, how e-mail links should be handled, templating, et cetera. Of course, we also support different sorting and grouping options. 

And here is where we get into the real topic of today's post.

For the staff directory, we needed to represent both sorting and grouping as a single option. It's all well and good when being fetched and processed, but when it comes to shipping it to FreeMarker, we had to do this:
<br>
{% highlight java %}
if (order.isGrouping()) {
    final Collection<Pair<String, List<User>>> sortedStaff = order.sort(staff);
    data.put("staffList", sortedStaff);
} else {
    final Collection<ClassicUser> sortedStaff = order.sort(staff);
    data.put("staffList", sortedStaff);
}
data.put("isGrouping", order.isGrouping())
{% endhighlight %}
<br>
Yeah... not great.

The idea here is that every template needs to support both possible types of Collection (a list of users or a list of grouped users), while reusing most of the same infrastructure. Unfortunately, that means we gotta get a little messy on the backend.

We need to know *exactly* what we're getting back before moving on-- we can't just leave it at Collection<?> and call it a day! A little bit annoying, but I get it. We can't reasonably expect Java to accept any of two types for a declaration.

But, know what can? [Ceylon](https://ceylon-lang.org){:target="_blank"}, Red Hat's homebrewed JVM language.

It supports a myriad of useful features (quite a few will be *very* [familiar](https://ceylon-lang.org/documentation/1.2/tour/){:target="_blank"} to Kotlin devs!), but we're only concerned with [union types](https://ceylon-lang.org/documentation/1.2/tour/types/#union_types){:target="_blank"} for now.

(I want to apologize for my Ceylon beforehand!)
<br>
{% highlight ceylon %}
// Some SAM that can return EITHER a String or a Boolean
interface HasValue {
   shared formal String | Boolean get();
}

// Concrete implementation of HasValue that returns a Boolean
value booleanValue = object satisfies HasValue {
        get() => true;
};

// This implementation returns a String
value stringValue = object satisfies HasValue {
        get() => "string";
};

// Both foo and bar can expect either type!
String | Boolean foo = booleanValue.get();
String | Boolean bar = stringValue.get();
{% endhighlight %}
<br>
The basic idea here is that we can define methods that return any *one type* from a *list of types*. You can think of it as A OR B OR ..., where A can be a [sequence](https://ceylon-lang.org/documentation/1.2/tour/sequences/#sequences){:target="_blank"} of Strings, B can be a sequence of [tuples](https://ceylon-lang.org/documentation/1.2/tour/sequences#tuples){:target="_blank"}, so on and so forth.

Revisiting the staff directory hack, we could probably write something like this:
<br>
{% highlight ceylon %}
// Ceylon is ref immutable by default by the way!
User[] | [String, User][] sortedStaff = order.sort(staff)
data.put("staffList", sortedStaff);
data.put("isGrouping", order.isGrouping)
{% endhighlight %}
<br>
Obviously, I haven't tested this in production, but it's fun to think about! I've never really done a deep dive on type systems / theory, so I find this and [intersection types](https://ceylon-lang.org/documentation/1.2/tour/types#intersection_types){:target="_blank"} deeply fascinating. Additionally, I coincidentally encountered an article on [higher-rank and higher-kinded types](https://www.stephanboyer.com/post/115/higher-rank-and-higher-kinded-types){:target="_blank"} while browsing the [Rust subreddit](https://www.reddit.com/r/rust){:target="_blank"}. Strangely enough, it was written in a Java-esque fashion, but that actually helped me grasp the core concepts very easily. 

As I explore these ideas, I'm wondering if more languages could benefit from such rich typing. It might seem a bit convoluted at first, but when you see them in action you get a sense of elegance and purpose. Maybe I should try programming in Ceylon for a month or so, it could be a fun little experiment!
