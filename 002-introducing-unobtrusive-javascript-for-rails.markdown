--- 
:format: :markdown
:title: Introducing Unobtrusive Javascript for Rails
:published_on: Tue Jun 06 22:23:00 UTC 2006
---
<div class="notice">Update 15 Feb 2009: The UJS plugin for Rails is no longer actively maintained but you can find the <a href="http://github.com/lukeredpath/ujs4rails">the source on github</a>.</div>

Rails makes a lot of things easier for a developer. One of those things is AJAX. The built-in Javascript and AJAX helpers, such as form\_remote\_tag and link\_to\_remote, make developing AJAX apps a breeze. But if there has every been one major bone of contention with these helpers, it's the markup they produce. Developers have been well aware of the need to separate content from presentation with CSS for a while now - less prevalent is the recognition of the benefits in separating *behaviour* from content.

**Update 21/08/2006**: The latest version of this plugin is 0.3 - please see [this post](http://www.lukeredpath.co.uk/2006/8/21/ujs-rails-plugin-0-3-new-name-new-website) and the [official UJS website](http://www.ujs4rails.com) for more information.

Rails makes a lot of things easier for a developer. One of those things is AJAX. The built-in Javascript and AJAX helpers, such as form\_remote\_tag and link\_to\_remote, make developing AJAX apps a breeze. But if there has every been one major bone of contention with these helpers, it's the markup they produce. Developers have been well aware of the need to separate content from presentation with CSS for a while now - less prevalent is the recognition of the benefits in separating *behaviour* from content.

I'm not going to cover the ins and outs of why you should separate behaviour from content here, but or those unfamiliar with the reasons and benefits, Peter-Paul Koch of [QuirksMode](http://www.quirksmode.org)	published a [great article](http://www.digital-web.com/articles/separating_behavior_and_presentation/)	in 2004 highlighting the benefits of separating behaviour from content. More recently, Dan Webb from [Vivabit](http://www.vivabit.com) published [an article about the problem with Rails AJAX helpers](http://www.vivabit.com/bollocks/2006/02/09/rails-is-the-devil-in-your-client-side-shoulder).

As Dan points out in his article, the subject has been mentioned on [several](http://thread.gmane.org/gmane.comp.lang.ruby.rails/15668) [occasions](http://lists.rubyonrails.org/pipermail/rails-spinoffs/2005-June/000025.html) on the RubyOnRails mailing list but has often been shot down for a multitude of reasons ranging from "its not worth it" to "it would be too awkward". Invitations to submit patches were made but nothing came about.

So I'd like to present to you something I've been working on here at [Agile Evolved](http://www.agileevolved.com) - **[Unobtrusive Javascript for Rails](http://opensource.agileevolved.com/trac/wiki/UnobtrusiveJavascript)**. The plugin makes use of a Javascript library called [event:Selectors](http://encytemedia.com/event-selectors/) by Justin Palmer at EncyteMedia. Similar to Ben Nolan's [behaviour.js library](http://bennolan.com/behaviour/) but making full use of [Prototype](http://prototype.conio.net/), it allows you to use CSS selectors to attach Javascript events to your page. This plugin allows you to make use of the event:Selectors library, but in Ruby, directly from your controller or view and have the resulting behaviour rules dynamically generated at runtime *in an external javascript file*. Let me demonstrate:

First of all, you'll need to grab the plugin from the [Subversion repository](http://opensource.agileevolved.com/svn/root/rails_plugins/unobtrusive_javascript/trunk/). Please note - the plugin is in its early stages at the moment and is likely to be updated quite a bit over the coming weeks - I do not recommend using this in a production site just yet. In the meantime, if you want to have a play I recommend using svn:externals to make sure you keep your copy of the plugin up-to-date.

	$ ./script/plugin install -x http://source.ujs4rails.com/current/unobtrusive_javascript

Next, you'll need to load in the Prototype javascript and the Unobtrusive Javascript scripts. Somewhere between your layout's head tags, add the following:

	<%= javascript_include_tag :defaults %>
	<%= unobtrusive_javascript_files %>

You can now attach events to elements in your page from either your controller or your view, using the register\_js\_behaviour() function. The function takes two parameters; the first is the CSS selector and event (for more details see the [event:Selectors documentation](http://encytemedia.com/event-selectors/)) to attach your behaviour to and the second is a string of Javascript that you want to execute. Here's a small example:

	<% register_js_behaviour "#my_funky_link:click", "alert('Hello World')" %&gt;

Of course, writing out large strings of javascript could become cumbersome. The first solution is use the [built-in Rails helpers](http://api.rubyonrails.org/classes/ActionView/Helpers/PrototypeHelper/JavaScriptGenerator/GeneratorMethods.html) to generate your Javascript strings. For instance, if we want to highlight a div when we hover over it with the mouse, you could do the following:

	<% register_js_behaviour "#my_funky_div:mouseover",
            visual_effect(:highlight, "my_other_div", :duration =&gt; 0.5) %>

Now, if you load up a page with one of the above calls and View Source, you might expect to see a whole load of generated Javascript. Except you won't - thats because the unobtrusive_javascript plugin makes use of a special controller and view to dynamically generate your behaviour rules at runtime, which are then linked to using a normal script tag. That means you can attach as many behaviours to your page as you like, from anywhere in your view or controller, *without* clogging up your rendered HTML with Javascript.

Next, there is the issue of graceful degradation. The two primary candidates for graceful degradation are the link\_to\_remote and form\_remote\_tag helpers - links and forms both have natural fallbacks - the HREF and ACTION respectively, and by using some unobtrusive Javascript we can make sure those fallbacks are used when somebody has Javascript disabled.

This initial release contains an updated version of the form\_remote\_tag helper - by default it does exactly the same thing as the built-in Rails helper but making it use unobtrusive Javascript is a case of one small addition to your code:

	<%= form_remote_tag :url => { :action => 'foo' }, :unobtrusive => true %

Because the event:Selector library depends on an HTML ID to attach an event, your forms will need an ID but worry not - another modification to the form\_remote\_tag helper will mean that if you do not specify your own HTML ID, one will be automatically generated for you.

Finally, some of you may be aware that Dan Webb, who I mentioned earlier in this article, has been working on his own [unobtrusive javascript plugin](http://svn.vivabit.net/external/rubylibs/unobtrusive_javascript/). I have been in touch with Dan and he preferred the simpler syntax that my plugin uses. That said, he's plugin contains some very cool stuff and we've agreed to merge the best of both of our plugins together over the coming weeks, including the ability to supply the register_\javascript\_behaviour() function with a block which will let you write the Javascript functionality you want to attach using Dan's RJS-style JavascriptProxy classes.

Please do download the plugin and have a play around. You can report any bugs on the [Agile Evolved Open Source Software website](http://opensource.agileevolved.com) and any opinions and ideas are welcomed.