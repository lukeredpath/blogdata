--- 
:format: :markdown
:title: Unobtrusive Javascript broken in Edge Rails, small fix release
:published_on: Thu Aug 10 12:26:00 UTC 2006
---
We're currently using Edge Rails and the Unobtrusive Javascript for Rails plugin in the forthcoming [Rails Plugin Repository](http://svn.lazyatom.com/public/plugin_repository/trunk/) and after a Subversion update today, something seems to have broken the Unobtrusive Javascript plugin.

I'm not sure what caused the breakage but the problem was a minor one; after some investigation it turned out that the unobtrusive javascript controller was not being automatically being required by Rails even though its parent folder had been added to the Rails load path. The fix was as simple as explicitly requiring the controller file and everything is now fine. <del>[Release 0.2.1](http://opensource.agileevolved.com/svn/root/rails_plugins/unobtrusive_javascript/tags/rel-0.2.1/)</del> addresses this issue and also adds an about.yml file containing plugin meta-data.

**Update**: <ins>[Please grab Release 0.2.2 instead](/index.php/2006/08/10/unobtrusive-js-022-the-two-in-one-day-release/).</ins>

If you are running on Edge Rails you will need to upgrade to this release. The plugin (version 0.2) continues to work just fine if you are running the new [Rails 1.1.5 security fix](http://weblog.rubyonrails.org/2006/8/9/rails-1-1-5-mandatory-security-patch-and-other-tidbits) so whatever caused this breakage doesn't appear to be related to the security fix.