--- 
:format: :markdown
:title: Update to Unobtrusive Javascript Plugin
:published_on: Wed Jun 07 14:33:00 UTC 2006
---
Some small changes and one major bug fix have been checked into Subversion today:

* The controller action that serves up event-selector.js, eliminating the need to manually copy it to your public/javascripts folder, was looking for the js file in the wrong place, stopping the whole plugin from working. If you've already discovered this, **please run an update**.
* There is now an unobtrusive version of the link\_to\_remote function - this also dynamically appends a return false to the onclick event to ensure it works in Safari (see [this blog post](http://particletree.com/notebook/eventstop/))
* The rules are now assigned using an addLoadEvent function instead of window.onload - this is more of a quick fix and will be improved in the future (I plan to use Prototype's onReady extension) but in the meantime this makes sure it doesn't mess up any other onload events you might have.
* The rules are re-applied automatically on completion of any AJAX request

Please post any bugs to the [Agile Evolved Open Source Trac](http://opensource.agileevolved.com/).