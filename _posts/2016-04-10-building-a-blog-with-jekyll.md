---
layout: post
title: Building a Blog With Jekyll
date:   2016-04-10 12:38:00 -0400
categories: jekyll
---

Let me first take this opportunity to utter that famous computer science phrase,
"Hello world!"

I recently decided to build my own blog for a few reasons:

* To take part in the online conversation about things I care about
* Writing about things that I learn can only help solidify new concepts (you
  can't teach what you don't know)
* To help anybody out there who faces similar issues to me
* For my own entertainment
* To learn how to run my own website

As far as the direction of the content on this blog, it will certainly be very
tech-heavy. I do have varied interests though, so expect different types of
posts to pop up. Even if they are not your particular cup of tea, I still hope
that you find this blog useful and/or entertaining.

This first post is about how I built this blog. The friendly folks at [upcase][upcase]
recommended [Middleman][middleman] as a static site generator, and with good
reason - by all accounts, it is a very powerful and flexible site generation
framework. Unfortunately, I had difficulties setting it up. Specifically, I
got this error

```
/home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/specification.rb:2274:in `check_version_conflict': can't activate addressable-2.3.8, already activated addressable-2.4.0 (Gem::LoadError)
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/specification.rb:1403:in `activate'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:89:in `block in require'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:88:in `each'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:88:in `require'
  from /home/user/.gem/ruby/2.3.0/gems/addressable-2.4.0/lib/addressable/template.rb:20:in `<top (required)>'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.gem/ruby/2.3.0/gems/middleman-core-4.1.6/lib/middleman-core/util/uri_templates.rb:3:in `<top (required)>'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.gem/ruby/2.3.0/gems/middleman-core-4.1.6/lib/middleman-core/util.rb:12:in `<top (required)>'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.gem/ruby/2.3.0/gems/middleman-core-4.1.6/lib/middleman-core.rb:13:in `<top (required)>'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.rubies/ruby-2.3.0/lib/ruby/2.3.0/rubygems/core_ext/kernel_require.rb:68:in `require'
  from /home/user/.gem/ruby/2.3.0/gems/middleman-cli-4.1.6/bin/middleman:12:in `<top (required)>'
  from /home/user/.gem/ruby/2.3.0/bin/middleman:22:in `load'
  from /home/user/.gem/ruby/2.3.0/bin/middleman:22:in `<main>'
```

when I ran `middleman init`. No idea how I could target addressable 2.3.8, since
I was not in a project directory with a Gemfile. By downgrading the system gem
perhaps?

Anyway, a day or two later, I saw this link on [HackerNews][gitlab-pages] (I
warn you, the comments are a cesspool. Stay away, for your own health). Look
at that, GitHub's young upstart competitor has native support for middleman
built right in! So yesterday, I forked their [example repo][gitlab-middleman]
and with the [middleman-blog][blog-gem] gem got to a minimal, working blog.

Awesome, now let's deploy it... Umm... I had some difficulties with this step.
I tried deploying to GitHub with the [middleman-gh-pages][middleman-gh-pages]
but for some reason or another could not get the site to show up (yes, I
[RTFM][rtfm]'d). Then I tried deploying to Gitlab pages. This can certainly be
done, but I was neither clever nor patient enough to wrap my head around their
Continous Integration system (Note to self: learn what continuous integration is).

Frustrated, I said to myself, "You know Panashe, you *could* just use Jekyll.
If you ever need more functionality, you can cross that bridge when you come to
it". [GitHub Pages officially supports Jekyll][gh-jekyll], so it could hardly be
more difficult than Middleman. In fact, it was extremely simple. Stupidly simple
even. And it's pretty damn well documented. Jekyll may not be as capable as
Middleman, but it certainly has everything I need for now.

While I will probably revisit Middleman in the future, Jekyll is simple and good
enough.

So without further adieu, I welcome you to my new blog, "I Write What I Like",
titled after the [book by Steve Biko][biko].

[upcase]: https://upcase.com
[gitlab-pages]: https://news.ycombinator.com/item?id=11430889
[gitlab-middleman]: https://gitlab.com/pages/middleman
[blog-gem]: https://github.com/middleman/middleman-blog
[rtfm]: https://en.wikipedia.org/wiki/RTFM
[biko]: http://www.amazon.com/Write-What-Like-Selected-Writings/dp/0226048977
[middleman]: https://middlemanapp.com/
[gh-jekyll]: https://help.github.com/articles/using-jekyll-as-a-static-site-generator-with-github-pages/
[middleman-gh-pages]: https://github.com/edgecase/middleman-gh-pages
