--- 
:title: Unobtrusive Javascript in Rails...what if?
:published_on: Tue Jun 06 00:22:00 UTC 2006
---
<div class="notice">Update 15 Feb 2009: The UJS plugin for Rails is no longer actively maintained but you can find the <a href="http://github.com/lukeredpath/ujs4rails">the source on github</a>.</div>

What if...you could produce accessible, unobtrusive javascript, using Rails built-in javascript/prototype helpers, with just one extra line of code in your layout, a plugin, and one small enhancement to the helpers? Something like this:

	<% form_remote_tag :controller => 'foo', :action => 'bar', :unobtrusive => true %>

Which produces:

	<form id="form_foo_bar" action="/foo/bar" method="post">

But which still acts as an ajax form? You would simply handle the response for ajax and non ajax requests using Rails' responds_to function.

In addition, what if you could attach javascript functionality to your page elements in a Behaviour-like fashion, but using pure Ruby, anywhere in your view, but loaded from an external Javascript file?

Sorry to tease. Stay tuned, we have something very cool to show off in the next day or two.

[http://opensource.agileevolved.com](http://opensource.agileevolved.com "Agile Evolved Open Source Software")