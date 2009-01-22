--- 
:format: :markdown
:title: Rails Plugin Packs&trade; coming to RaPT
:published_on: Fri Jul 28 17:00:00 UTC 2006
---
Good news - [Geoffrey](http://www.topfunky.com) has agreed to roll Rails Plugin Packs into RaPT. I've already integrated it into the RaPT source code and a patch is winging its way to Geoffrey's inbox as I write this. 

For the adventurous who want to give it a try in the meantime (with all the usual caveats), you need to first check out the RaPT source:

    $ svn co svn://rubyforge.org/var/svn/rapt/trunk rapt

<del>Next, [download the pluginpack integration patch](http://www.lukeredpath.co.uk/uploads/ruby/add_plugin_packs_to_rapt.diff). Change to the rapt directory you just checked the source out to and apply the patch.</dl>

<ins>**Update**: I've been added to the RaPT committers list; this patch has now been applied to the trunk. Once you've checked it out, you simply need to build and install the gem.</ins>

    $ rake package
    $ gem install pkg/rapt-0.1.0.gem

You'll find RaPT will now have three new commands: pack:install, pack:uninstall and pack:about. All three commands take one argument, the system path or URL to your pack file. Run rapt -h for more information.
