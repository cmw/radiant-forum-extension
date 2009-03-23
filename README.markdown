# Forum

This is a simple but clean forum implementation that plugs into radiant. It's fairly well-featured, including 

* rss feeds at several levels
* email monitoring of topics
* moderation by readers and radiant users (soon)
* message attachments (soon)
* message preview
* textile-formatted messages with html whitelisting

but I've tried to keep it clean and simple on the inside. You should find it very easy to adapt and extend. 

The forum was derived long ago from Beast and the bones will still be visible here and there. It has been in use on various sites in various versions for the last two years, but in consolidating it here and making it ready for publication I've pretty much refactored the whole thing. Some of it is quite new, then, and may have exciting new bugs in it. If I were you I'd give it a few days to settle down. It's properly specced and tests clean, though.

The forum is compatible with multi_site but you have to use our fork (https://github.com/spanner/radiant-multi-site-extension) if you want forums and readers site-scoped.

## Still to do

* Message-editing in the main readers' view and topic moderation in admin
* Message attachments
* Sample javascript front end (in mootools and possibly also prototype/jquery)
* Moderators
* Bad message button
* Admin pages that suck less
* More comment-formatting tags

## Installation

At the moment Ray doesn't know about these extensions, so you will need to jump through some hoops to install reader (qv), but after that the forum is very simple:

	git submodule add git://github.com/spanner/radiant-forum-extension.git vendor/extensions/forum
	rake radiant:extensions:forum:migrate
	rake radiant:extensions:forum:update

In future this ought to be all you need:

	rake rake ray:extension:install name="forum"

The update task brings in a basic stylesheets/forum.css that you will want to improve upon, an admin/forum.css that you can probably leave alone, and various useful images.

## Requirements

The reader extension (and therefore spanner's multi-site fork) and share_layouts. For the (not yet) message attachments you will one day need paperclipped. Ray should pick up the dependencies but I don't think it'll get the forked multi-site yet.

## Load order

For best results, you will want this in your environment.rb

	config.extensions # [ :share_layouts, :multi_site, :reader, :all ] 

## Administration

The forum is very simple to use and almost entirely separate from your page hierarchy. Forums - which are described as 'discussion categories' most of the time and pushed into the background - are created and edited in the admin interface, and there readers can also be promoted to moderate selected forums. Everything else happens through the reader interface.

## Page Comments

To enable page comments, all you have to do is put `<r:comments />` somewhere in the layout (or in a snippet or even on an individual page). Behind the scenes this will create a dedicated forum for page comments and a topic for each page as it is commented upon. The rest of the mechanism is the same, and the comments themselves are just posts. Because this is done through radius tags you don't get much control over comment formatting at the moment. I'll break it down further at some point so that you can place the parts yourself. 

## Security & Spam

The reader extension includes email confirmation and a simple honeytrap that should prevent most bots from registering. The post forms include proper XSS protection via the old white_list plugin and the usual CSRF protection is enabled. I probably need to go through making sure every is h'd that should be.

## Ajax

I've published this in the simplest form possible, so as to get the basics in place and ready for customisation. This means - for example - that clicking on 'add a comment' on a page is just a get to a new page with the comment form on it. It's not very user-friendly to lose context that way but it's just a framework: in production I always put some javascript in front of it so that clicking on that comment link would bring the comment (or login) form directly into the page. I haven't included that interface code yet because I prefer to use mootools and I'm not sure about imposing yet another javascript framework on people. Let me know if you want it. The hooks are all there in the controllers (for a simple lumps-of-html implementation, anyway. If you want to do it properly without passing layout around it might need thumping a bit).

## Smiles

The people love smilies. How they love them. I've included a basic set here and a redcloth extension that catches :smile: notation alongside the textile markup. There's a javascript front end for that too - click on the smiley, you know - which will soon appear here, along with the rest.

## Author & Copyright

William Ross, for spanner. will at spanner.org
Originally based on dear old Beast, currently not visible at [http://beast.caboo.se](http://beast.caboo.se)
Copyright 2007-9 spanner ltd
released under the same terms as Rails and/or Radiant