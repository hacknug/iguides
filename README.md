smpl - simpliest jekyll theme
=============================

Easiest (sic!) way to run blog like a hacker using Github Pages.

[Github  Pages][github-pages]   is  a   cool  service.   It  provides   a  free
hosting   for   static   websites   which    is   a   great   choice   to   any
project  to  host   its  documentation.  Even  better  is   that  it  [provides
ability][github-pages-jekyll-usage] to use  great static websites **generator**
named Jekyll  to introduce some  kind of dynamics and  drop the need  to create
each separate html-page by your hands (or need to generate a site locally).

While you creating content for the website you  do not need to use less or more
poky WYSIWYG-editor, to  have an browser or even graphical  environment at all!
Content are just  simple text files, templates are also  text files and changes
sending is  not a form submit  using website interface, but  regular well-known
push to git repository!

Furthermore, Github  now provides full [C][]R[U][][D][]  support for everything
described above! I am  using it only for little bugs fixing,  but you still can
use it as full WYSIWYG!

Altogether,  each cool  hacker  runs its  blog  like  this. And  I  am not  the
exception! But  I didn't like  nether default Jekyll's  theme nor one  from the
hundreds of available  themes. They didn't have... minimalism,  even those that
had "minimalistic" word in their names.  Minimalism not only in their look, but
also  in what  is under  the  hood: within  templates, scripts,  stylesheets...
Because of  this I decide  to make my own  theme with blackjack  and everything
else. And main requirement was simplicity in everything described above.

I am not a designer and I not  like to design things, but - wow! - minimalistic
design,  when  content is  placed  at  the  forefront,  i. e.  "content  first"
approach, on  the one  hand is  more comfortable for  a reader  (simply because
there  is no  distracting  gimmicks), but  on  the  other hand  it  is easy  to
implement.

*Templates*:  why do  we need  a lot  of partials?  Let's make  just one,  easy
modifiable and  understandable! *Stylesheets*: only base  simple layout, styles
for default  elements let's leave  to a browser  - simply because  in different
browsers they differs only by values of margins and paddings. *Scripts...* what
the scripts, for what?! Why do we need any kind of dynamics within simple - and
trying to keep its simplicity! - blog?

So,  how  to  use  `smpl`?  Minimum  files  set  which  you  need  to  have  in
blog's  repository  consists  in  `_config.yml`  (main  website  settings,  you
better to  look inside),  `index.html` (main  - and the  only! -  template) and
`styles.css` (the  minimum set  of styles).  We need  to create  git repository
named `<your-nickname-on-github>.github.io`,  put within  its root  these three
files, that may be downloaded from  [theme repo][theme-repo] - and we are ready
to start writing posts! Posts (written in [markdown][github-flavored-markdown])
need to  lay within the  `_posts` directory, be  named with respect  to [Jekyll
requirements][jekyll-post-requirements]  (YYYY-MM-DD-post-title.md)   and  must
contain minimal [front-matter][jekyll-front-matter]:

```yaml
---
layout: index
title: A post title
---
```

Further please refer to [Jekyll documentation][jekyll-docs]. At this moment you
already may make a git push and take a look at your nice minimalistic blog.

Optionally  you also  may  copy  files named  `404.html`  and `atom.xml`.  They
service to generate custom 404 error  page (a warning plus redirect to homepage
after five seconds of waiting) and atom-feed, respectively. If you want to have
your blog on  your custom domain you can refer  to corresponding [documentation
entry][github-pages-custom-domain] of Github Pages.

You may  take a look  at theme  and appreciate the  charm of its  minimalism on
[this website][demo], and if you want to see underlying structure you may go to
[theme repo][theme-repo].



[github-pages]: https://pages.github.com/
[github-pages-jekyll-usage]: https://help.github.com/articles/using-jekyll-with-pages
[github-flavored-markdown]: https://help.github.com/articles/github-flavored-markdown
[github-pages-custom-domain]: https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages
[C]: https://github.com/blog/1327-creating-files-on-github
[U]: https://github.com/blog/143-inline-file-editing
[D]: https://github.com/blog/1545-deleting-files-on-github

[jekyll-post-requirements]: http://jekyllrb.com/docs/posts/
[jekyll-front-matter]: http://jekyllrb.com/docs/frontmatter/
[jekyll-docs]: http://jekyllrb.com/docs/home/

[demo]: http://neoascetic.me
[theme-repo]: https://github.com/neoascetic/neoascetic.github.io
