--- 
:format: :markdown
:title: Updating a single record attribute with ActiveRecord
:published_on: Wed Jun 28 11:15:00 UTC 2006
---
On a Rails project I'm working on at the moment, the Rails app had to make calls to a third-party application using web services - we would then update certain attributes on a particular model instance with data returned by the web service. The only problem was that the third-party app had to update (different) attributes on the same record. Whilst the web service would always respond in a timely fashion, there was no way of predicting when the third-party app would run its update (it could be delayed if the server the server was under heavy load).

This resulted in a race condition that could potentially result in a loss of data. Because Rails already had a loaded instance of the object in memory before the web service call was made, if the third-party app saved its changes before Rails saved the record with its own changes, the values already loaded in the Rails object would overwrite the new values written by the third-party ap

The solution seemed obvious at first - instead of doing:

	obj.some_attribute = 'foo'
	obj.save

We could simply do:

	obj.update_attribute('some_attribute', 'foo')

 But we were mistaken. Whilst the method's name would seem to imply that ONLY a single attribute would be updated, this is in fact not the case. All update_attribute() does is exactly the same thing as the first code snippet. From the Rails source:

	def update_attribute(name, value)
	  send(name.to_s + '=', value)
	  save
	end

 As it turns out, there isn't a built-in way of updating just a single attribute that we could see. So we wrote our own, simple solution:

	def update_single_attribute(attribute, value)
	  connection.update(
	    "UPDATE #{self.class.table_name} " +
	    "SET #{attribute.to_s} = #{quote(value)} " +
	    "WHERE #{self.class.primary_key} = #{quote(id)}",
	    "#{self.class.name} Attribute Update"
	  )
	end

Feel free to use this in your own apps if needed - the easiest way to use is it is as a [monkey patch](http://en.wikipedia.org/wiki/Monkey-Patch) to ActiveRecord - simply paste the following code into a file called active\_record\_patches.rb in your app's lib folder and require it at the bottom of your environment.rb file. Alternatively, you could patch the Rails source directly or convert this to a Rails plugin.