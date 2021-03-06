% adieu, Jekyll (i dumped you for a 28 lines Makefile)

Months ago, and once again, i spent hours to figure out how to do things i
expected to be simple. In this case, Jekyll isn't the one to blame: the ruby
ecosystem is.  I'm a happy [CPAN](https://metacpan.org/) user and contributor
as well as [cabal](http://www.haskell.org/cabal/) happy user. Coming from those
world, the ruby one is quiet messy.

So i decided to throw all the blog/wiki engines away, running a temporary
solution with the simplest bootstrap i can write. Following the rules of the
unix [KISS principles](http://en.wikipedia.org/wiki/KISS_principle), i divided
the "CMS" problem more little ones. I needed something as simple as possible to
extend coming with: 

* a content generator
* a conductor to drive it
* a responsive design
* a web browsable SCM 
* an atom generator
* a discus-like comment system

so i choose the simplest (or best) tools i know for each task:

* [pandoc](http://johnmacfarlane.net/pandoc/) to generate html content from md files.
  I don't know about the internals but both the CLI tool and the haskell API are very pleasant
  to use. 
* [make](http://pubs.opengroup.org/onlinepubs/000095399/utilities/make.html) is
  my conductor. As i really expect the every so-called unix users have the
  basics of make. It get the job done for many decades now. I also have a look on 
  mk from [the 9base](http://tools.suckless.org/9base). 
* [git](https://git.wiki.kernel.org) as SCM, lot of tools out there to browse it from the web.
* [Unsemanic CSS framework](http://unsemantic.com/). I don't know a lot about
  web development but i was aware about
  [Responsive web design](http://en.wikipedia.org/wiki/Responsive_web_design)
  and saw the ancestor of unsemantic was in the roadmap of 
  [werc](http://werc.cat-v.org/) (when it comes to simplicity, you really can trust the 
  [cat-v](http://cat-v.org/) Ayatollahs^wpeople)
* more recently, i wrote [atombomb](https://github.com/eiro/app-atombomb) to add
  atom feeds to some sites.

so the workflow is:

* create a new md for a new page
* manually maintain the atom feed (
  [atombomb format](https://github.com/eiro/eiro.github.com/blob/master/Makefile)
  is quiet helpfull for this part)
* run make to build a section
* git submodules to add subsection
* rsync or git to push on the server

you can see all the revision by using a git browser (like the github one) and i
plan to use an ajax call the powerfull [sympa list manager](http://sympa.org)
to run a discus-like comment system.

Months passed and it became clear i will not step back: those 62 LOC (it takes
less than 5 minutes to understand the whole thing) never desapointed me. 

* the publishing process is faster and easier
  * i use the :make command of vim
  * my working directory is served by a local http server
  * everything is relative to the working directory:
    if it renders well here, it will render well offline. 
faster and easier local preview before pushing
* so easy to extend i never was stuck by a new problem, i just had to write
  some few extra lines in the Makefile to get new features like
  * [graphviz](http://www.graphviz.org/) and [ditaa](http://ditaa.sourceforge.net/)) support
  * beamer slides (using the theme of my university).
  * render html report from external sources (just use pandoc md as
    intermediate representation and run make)

Actually, even my todolist/notes system is now based on it (in combination with
vim and mutt i'll explain in another post) and as always, i realized how happy i am when i 
follow the rules of the unix [KISS principles](http://en.wikipedia.org/wiki/KISS_principle). 

<pre>
    # wc -l M* t* 
      28 [Makefile](https://github.com/eiro/eiro.github.com/blob/master/Makefile)
      27 [template.html5](https://github.com/eiro/eiro.github.com/blob/master/template.html5)
       7 theme.css
      62 total
</pre>

the only thing i was afraid was "all is relative to the working directory"
thing: i copy the css files for each new section. suprisingly, it has benefic
effects. 

* i think twice before adding a section and it keep me aware of the content. 
* the local CSS actually import a stylesheet shared by all the sites i manage so i can easily  
  maintain the whole stuff.

So another unix principle is at work here: "Premature optimization is the root
of all evil". Thanks Donald!

#(2014-01-18T22:08:52+01:00) atom bomb first test

i decided to push a repository on github with all those tests and notes that
can maybe become a project some day and named it
[labo](https://github.com/eiro/labo/).

this feed is generated with 
[atom bomb](https://github.com/eiro/app-atombomb) and the
[feed file](https://github.com/eiro/app-atombomb/blob/master/t/feed) i just maintain
manually with vim and [simple zsh helper](https://github.com/eiro/uze/blob/master/atombomb)
to create headers.

atom bomb (i'm not sure about the name) is written in
[perl5](http://www.perl.org/) and
[Eirotic.pm](https://metacpan.org/pod/release/MARCC/eirotic-0.0/lib/Eirotic.pm) which
replace my common boilerplate:

* [Perlude](https://metacpan.org/release/perlude)
* [Method::Signatures](https://metacpan.org/pod/Method::Signatures)
* perl 5.14 with strictures
* YAML

markdown conversion is made by [pandoc](http://johnmacfarlane.net/pandoc/). 
