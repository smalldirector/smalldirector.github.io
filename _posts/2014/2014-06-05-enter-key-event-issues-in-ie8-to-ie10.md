---
layout: post
title: "Enter key event issues in IE8 to IE10"
categories: [Web, Javascript]
---

When I implemented the Enter Key event for input element, I found some strange things happend in IE8 to IE10.

These browser's input element cannot reponse Enter Key event, moreover, when you click Enter in the `<input>` field, it will click the `<button>` element in your page automatically.

How can we solve this problem, there are two ways:

* Wrap all the `<input>` elements in the `<form>`, and all the `<form>` should have a submit button.
* You can add `type="button"` to the `<button>` element, this way will override the default `type="submit"` and prevent IE from clicking the `<button>` on Enter Key event.

{% highlight html %}
<input type="text">
<button type="button"></button>
{% endhighlight %}


