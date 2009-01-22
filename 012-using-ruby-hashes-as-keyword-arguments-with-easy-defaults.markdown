--- 
:format: :markdown
:title: Using Ruby hashes as keyword arguments, with easy defaults
:published_on: Thu Jul 27 10:36:00 UTC 2006
---
Similar to many Rails helpers/methods, a lot of the methods I write often use an optional hash of options, or sometimes just a hash only, to simulate keyword arguments (often using symbols).

The only downside to doing this is you lose out on easily setting default values using Ruby's default method argument values. You might use code something similar to the following to make up for this:

	def some_method(opts={})
	  my_foo =  opts[:foo] || 'mydefaultfoo'
	end

However, as you have more and more keyword options, setting defaults in this way gets rather tedious. Fortunately, Ruby's Hash#merge comes to our rescue (almost) - it allows you to merge the contents of one hash with another. The only problem - any duplicate keys in the hash you are merging will overwrite your original hash values - when it comes to setting default values, we want this to work the other way around; we only want values in the defaults hash to be merged if they do not exist in the original hash. Again, Ruby comes to our rescue - Hash#merge takes a block as an argument and will pass any duplicate values that crop up into the block - we can use this block to decide which value to keep.

Using the simple monkey patch to the Hash class below, you will no longer have to set each default individually:

	class Hash
	  def with_defaults(defaults)
	    self.merge(defaults) { |key, old, new| old.nil? ? new : old } 
	  end

	  def with_defaults!(defaults)
	    self.merge!(defaults) { |key, old, new| old.nil? ? new : old }
	  end
	end

Of course, sticking with Ruby naming conventions, with\_defaults() will return a new hash whilst with\_defaults!() will change the original hash directly. Now all you have to do is something like this:

	def my_funky_method(opts={})
	  opts.with_defaults! {
	    :arg_one => 'foo',
	    :arg_two => 'bar',
	    :arg_three => 'baz'
	  }
	end

**Update**: [Dan Web](http://www.vivabit.com/bollocks/) just informed me of another way of doing this - instead of merging your defaults into your options, simply merge your options into your defaults et voila!

	def my_method(opts={})
	  {:arg_one => 'foo', :arg_two => 'two'}.merge!(opts)
	end

If there is one thing that is annoying about Ruby, it's that just when you think you've come up with a cool little code snippet, somebody inevitably comes up with an even easier way of doing the same thing. 

That said, I do still think there is something nice about the with_defaults() method - it's slightly more intention-revealing. Weigh that up with the need to monkey-patch. Choose what works best for you.

**Update 2**: Josh Susser now informs me (see comments) that this functionality is built-right into Rails courtesy of [ReverseMerge](http://api.rubyonrails.org/classes/ActiveSupport/CoreExtensions/Hash/ReverseMerge.html). I still think with_defaults sounds better but I guess that pretty much makes this post redundant!