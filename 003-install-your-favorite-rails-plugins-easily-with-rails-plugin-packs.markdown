--- 
:format: :markdown
:title: Install your favorite rails plugins easily with Rails Plugin Packs&trade;
:published_on: Thu Jul 27 12:21:00 UTC 2006
---
After a brief brainwave on the way home from work, I came up with a simple solution to the problem, which I now present to you simply as: Rails Plugin Packsâ„¢.

The concept is incredibly simple; you write your own plugin pack - nothing more than a simple specification written in YAML - and publish it somewhere, either storing it somewhere on your harddrive, on your network or hosted on your website. You then use a series of simple Rake tasks to install/uninstall these packs by simply specifying the URL/path to the pack file.

I've initially implemented this as a Rails plugin, so install it to get going:

	$ ./script/plugin install http://opensource.agileevolved.com/svn/root/rails_plugin_pack

This will give you three new Rake tasks - pluginpack:install, pluginpack:uninstall and pluginpack:about. What they do should be pretty self-explanatory. They all take a pack url/path as an argument, in the form PACK=url\_or\_path, for example:

	$ rake pluginpack:install PACK=http://www.lukeredpath.co.uk/mygreatpack.pluginpack

Currently, Subversion is required to use the plugin as it installs each plugin by doing an svn co of each plugin. I plan to expand on the ways of installing the plugins soon, including support for svn export and svn:externals.

Writing your own pluginpacks is simple - the file extension can be whatever you like although I prefer to use the standard .pluginpack suffix. The files themselves are very simple and are written in [YAML](http://www.yaml.org/). Here's an example plugin pack file (which comes bundled with the rails\_plugin\_pack plugin itself):

	about:
	  name: Example Pack
	  description: An example plugin pack
	  author: Luke Redpath
	  email: contact@lukeredpath.co.uk
	  website: http://www.lukeredpath.co.uk
	plugins:
	  unobtrusive_javascript: http://opensource.agileevolved.com/..[truncated]
	  navigation_mappings: http://opensource.agileevolved.com/..[truncated]

All of the meta-data is optional, although if you omit a name it will result in your plugin being called "Untitled Plugin" so I recommend giving every one of your packs a name, at least.

So what possible uses could this have? I've already discussed facilitating the installation of the same group of plugins on each new project - you could have your own personal or company-wide "standard" plugin pack that you use on each new project. You could write a blog post about your favorite Rails plugins and offer a plugin pack containing each plugin discussed in your article. You could use it to group related plugins. You could even use it as a basic dependency mechanism, by offering packs of plugins that depend on each other (although this is no substitute for a proper dependency mechanism).

The only downside to this at the moment of course, is that Rails Plugin Packs is implemented as a Rails plugin itself, meaning you still need to install at least one plugin manually before you can do anything else on a new project. This isn't ideal and I'm waiting to hear back from [Geoffrey Grosenbach](http://topfunky.com) about the possibility of submitting a patch to RaPT and rolling plugin-pack functionality into the RaPT core.

Comments are open so as usual, all feedback welcome and appreciated; please file any bugs/issues in the [Agile Evolved Trac](http://opensource.agileevolved.com).

**Update**: I've made a small change to the plugin pack format so if you've already checked out the plugin you'll need to run an update. I've updated the example above and you can view the actual changes with the [trac changeset viewer](http://opensource.agileevolved.com/trac/changeset/158).