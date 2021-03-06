--- 
:format: :markdown
:title: "Release: Unobtrusive Javascript For Rails 0.2"
:published_on: Tue Aug 01 03:17:00 UTC 2006
---
<div class="notice">Update 15 Feb 2009: The UJS plugin for Rails is no longer actively maintained but you can find the <a href="http://github.com/lukeredpath/ujs4rails">the source on github</a>.</div>

[Dan Webb](http://www.vivabit.com/bollocks) and I are happy to announce the latest release of the Unobtrusive Javascript plugin for Rails. This release packs in lots of cool new features, a few changes to old ones and some performance enhancements.

**Update 21/08/2006**: The latest version of this plugin is 0.3 - please see [this post](http://www.lukeredpath.co.uk/2006/8/21/ujs-rails-plugin-0-3-new-name-new-website) and the [official UJS website](http://www.ujs4rails.com) for more information.

As I mentioned in [my previous article](http://www.lukeredpath.co.uk/index.php/2006/06/06/introducing-unobtrusive-javascript-for-rails/), Dan had been working on his own plugin and we've now pooled our efforts and are working on the single plugin together, with much of Dan's JavascriptProxy functionality from his own plugin now rolled into Unobtrusive Javascript for Rails 0.2.

Here's a full list of changes and additions:

* UPDATED: register\_js\_behaviour remains as an alias for backwards compatibility but is officially deprecated; it will probably be removed in the next release. There are also aliases for the American spelling of behaviour for our friends on the other side of the pond.
* NEW: You can now attach behaviours directly to elements using @tag@</pre> by passing in javascript event handlers in the HTML options hash (:onclick, :onmousedown etc.); they'll be extracted to the external behaviours file automatically.
* UPDATED: The core Rails AJAX/Javascript helpers (<code>link\_to\_remote, button\_to\_function, link\_to\_function, form\_remote\_tag</code> etc.) now work out of the box.
* NEW: There is no need to explicitly specify an HTML ID for the elements you want to attach behaviour to - if you don't, one will be generated automatically for you.
* NEW: Options to render behaviour rules directly in your page inside script blocks instead of in the external behaviour file.
* FIXED: Behaviours inside AJAX-loaded partials will now work.
* UPDATED: event:Selectors and domReady javascript libraries are replaced with the lowPro.js library by Dan Webb
* UPDATED: Javascript behaviours now have access to a single variable - @this@
* UPDATED: Behaviours can now cancel the default action by returning false as well as using Event.stop(event). This also works properly in Safari.
* FIXED: The required javascript files will be copied to your public folder automatically on installation. There is also a rake task for copying files across when upgrading the plugin.
* NEW: Documentation!

There is currently only one known issue:

* Behaviours are not being reapplied after an AJAX request in Opera 9

Here's a look at some of the new features, starting with writing behaviours using pure Ruby:

	<% apply_behaviour "#mylink:click" do |page, element|
	  page.alert "You clicked me! Now watch me fade..."
	  element.visual_effect :fade
	end %>

Attaching behaviours directly to elements with <code>content\_tag</code>

	<%= content_tag "div", "My funky box", :onclick => "alert('Hello World!')" %>

We want you to really put this release through its paces - does it conflict with other plugins? Does it cause unexpected problems with Rails itself? Are there any other features you'd like to see? The place to report these feature requests and bugs is of course [the Agile Evolved Open Source Trac](http://opensource.agileevolved.com/trac/newticket).

### Further Information

* [Unobtrusive Javascript for Rails Subversion URL](http://opensource.agileevolved.com/svn/root/rails_plugins/unobtrusive_javascript/tags/rel-0.2)
* [Plugin Documentation](http://opensource.agileevolved.com/unobtrusivejs/)

**Installation note**: some people have [reported](http://opensource.agileevolved.com/trac/ticket/9) that the plugin fails because the Rails plugin installation script installs the plugin to rel-0.2 instead of unobtrusive\_javascript. This is a failing of Rails' plugin installation system in that it doesn't correctly handle the typical tags/trunk/branches Suvbersion repository layout. If you install the plugin using script/plugin, make sure you rename the rel-0.2 folder to unobtrusive\_javascript. If you are using svn:externals, you can edit your externals file manually:

	$ svn propedit svn:externals vendor/plugins

Then add the following line to it:

	unobtrusive_javascript http://opensource.agileevolved.com/svn/root/rails_plugins/unobtrusive_javascript/tags/rel-0.2

### Future Plans

So what do we have planned for 0.3? More patches to Rails' more complicated built-in helpers, such as <code>apply\_behaviour</code> how about some of this:

	<% apply_behaviour "div.draggable", make_draggable %>
	<% apply_behaviour "li.sortable", make_sortable %>
	<% apply_behaviour "#searchbox", make_autocomplete %>

Comments open!