--- 
:format: :markdown
:title: Testing your Rails views with Hpricot
:published_on: Fri Jul 07 13:22:00 UTC 2006
---
**Update: This extension is now available as a Rails plugin. See below for more details.**

[\_why](http://www.whytheluckystiff.net) released his great little [Hpricot HTML parser](http://redhanded.hobix.com/inspect/okayGiveHpricot02AGo.html) the other day and the first thing that struck me would be that it would be a great tool for testing the output of Rails views. Rails comes with the built-in assert_tag method but I've always found it clunky and a pain in the arse to use.

By combining the Hpricot library and a few useful helper functions, you can now test the output of your Rails views in Test::Unit::TestCase easily using Hpricot's support for CSS selector searches.

First of all, install Hpricot if you don't have it already:

	$ gem install hpricot --source code.whytheluckystiff.net

Next, [download the Hpricot Test Extensions file](http://www.lukeredpath.co.uk/assets/2006/8/26/hpricot_test_extension.rb) and copy it to the lib folder of your Rails app.

Finally, require the file at the top of your test_helper.rb file:

	# test_helper.rb
	require 'hpricot_test_extension'

Alternatively, you can also install as a Rails plugin. The plugin is located in the [Agile Evolved Open Source repository](http://opensource.agileevolved.com/svn/root/rails_plugins/hpricot_test_helper/trunk).

And thats it. You'll now have access to four new methods - tag, tags, element and elements. All four methods take a CSS selector as an argument. The pluralised methods return an array of search results for the CSS query, the singular methods just return the first (handy if you know you'll only have one of a particular element, say, "title"). The tag methods return the inner contents of the returned elements, whereas the element methods return the raw Hpricot::Elem objects for you to work with. Here's a few examples:

	assert_equal "My Funky Website", tag('title')
	assert_equal 20, tags('div.boxout').size
	assert_equal 'visible', element('div#site_container').attributes['class']

Finally, there is a small extension to the Hpricot::Elem object itself which lets you assert the presence of text within an element using an alternative syntax:

	assert element('body').should_contain('My Funky Website')

I plan to integrate these helpers into [rSpec](http://rspec.rubyforge.org) as well using a more rSpec-style syntax. Any comments and suggestions are welcome.