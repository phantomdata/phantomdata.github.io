---
title: phantomdata's Rails Guide
author: Jordan T. Norlen
date: 2016/07/21
summary: A guide to rails development from a perspective of quality
layout: post
---

# Why Another Guide?

There are plenty of tutorials, books and videos on the subject of Rails and getting started.  Over my many years of development, I've learned that there's a hard difference between simply following a tutorial and actually learning what's going on with what you're doing.  Conversely, I've learned that there's a huge amount of intellectual masturbation that can go on if you dive too deeply.

I've compiled lots of little tidbits in various places around the Internet on topics ranging from setting up a new development machine to the best way to deploy your Rails application.  This article is an attempt to bring some of those back together in a single place.

Enjoy.

# Machine Setup

I work in a company with lots of different disciplines all comingling on a regular basis.  PHP, .NET, Java, Rails and Python...  we all get along pretty well.  We're also quite adaptable and end up going where the work is - which might no necessarily be in our favorite discipline.  The most common frustrations I see developers thrown into the Rails world is that of not having started from a good base.

You see, Ruby is pretty popular.  You might not know it, but a metric ton of the critical tools you use daily are built upon it.  For the most part, unless you're running Windows, Ruby is already installed.  ***gasp*** it was that easy?

Not quite.  That version that's installed is probably pretty old.  Not only that, but Ruby's ecosystem is actually pretty complex and diverse.  Over the years we've figured out ways to all work together - but most operating systems don't install Ruby that way.

If you use your system Ruby, you are probably going to have a bad time.  Follow below for steps to obviate that.

## Linux

Linux setup is pretty straight forward.  If you've tried other guides, make sure to examine and clear any remnants from your `~/.bash_profile` and `~/.profile`.

```sh
# Install basic developer tooling
sudo apt-get install build-essential libssl-dev git subversion imagemagick \
  libmagick-dev curl automake libpcre3-dev bison libmysqlclient-dev \
  libxslt-dev libpq-dev libreadline-dev libyaml-dev libcurl4-openssl-dev \
  postgresql postgresql-server-dev-9.1 git nodejs

# Install RVM to properly select Ruby versions
gpg --keyserver hkp://keys.gnupg.net \
  --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable

# Install Ruby and Rails (you may have to restart your terminal)
rvm install ruby 2.3.1
rvm use ruby 2.3.1
gem install bundler rails
```

## OSX

OSX setup is also pretty straight forward.  If you've tried other guides, make sure to examine and clear any remnants from your `~/.bash profile` and `~/.profile`.

```sh
# Install Homebrew so you can install other things
/usr/bin/ruby -e \
  "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Install a database and the most common image library
brew install imagemagick postgresql

# Install RVM to properly select Ruby versions
gpg --keyserver hkp://keys.gnupg.net \
  --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable

# Install Ruby and Rails (you may have to restart your terminal)
rvm install ruby 2.3.1
rvm use ruby 2.3.1
gem install bundler rails
```

## Windows

Don't.  Ruby and Ruby on Rails themselves run just great on Windows.  The problem comes from extended libraries that end up supporting the ecosystem.  They just don't play well on Windows.  If you must, use Vagrant to setup a developer box for yourself.  *Sorry for not detailing that here*

# Editor and Environment

How you run your actual minute to minute interactions with any discipline (except .NET) is a deeply personal choice.  Do you like light-weight editors with no frills?  Perhaps instead you prefer looming masses of scurrying robots in an IDE?  The choice is yours.  Its one that I highly encourage you to experiment with until you find just what is right for you.

That being said, I've outlined a few of the *common* choices below in alphabetical order.  I'm kind of an editor-junky and have about 25 different editors on my system right now, so don't think I haven't tried your favorite.  :)

## Atom

<a href="https://atom.io/" target="_blank">Atom</a> is a relative new-comer to the scene.  It's a semi-frills editor based around JavaScript.  Consequently, it can be kind of slow.  There are however a ton of plugins which can greatly aid your experience with Rails.  Once you've installed it, be sure to check some out.

## RubyMine

<a href="https://www.jetbrains.com/ruby/" target="_blank">RubyMine</a> is the hulking behemoth in the room.  I actually reach for this every time I'm opening a brand new code base to help me get my bearings.  Its got a ton of features and is absolutely awesome at helping you explore what you're working on.  This comes at a price though.  Its expensive, for one, but its also incredibly taxing on your CPU.  I certainly never run this when I have a MacBook on my lap.

## Sublime

<a href="https://www.sublimetext.com/3" target="_blank">Sublime</a> is the perpetual beta darling.  I'm fairly certain that they're never going to release an official 'stable' build of this editor.  Which is fine, because its rock solid.  Its a GUI based IDE that's easy to customize and navigate within.  There's not a whole lot going on, but it is *fast*, *stable* and *easy*.

## textMate

<a href="https://macromates.com/download" target="_blank">textmate</a> is the original Rails editor.  Its actually what made me switch over to OSX way back before I could grow a beard and had a bald spot.  Its a stable and fast editor that gave birth to the current crop.  Its biggest feature (imho) was command-p, which allowed you to simply hit command-p and type the name of the file you wanted.  Splendid.

An interesting note, although textmate seems to have had its development stopped and is no longer seen *anywhere* - it seems to be what DHH is still using.  I just spied it on the recent Rails 5 announcement video and was floored.  Dedication.

## Vim

<a href="http://www.vim.org/" target="_blank">Vim</a> is the grand-daddy of editors (*sssssh emacs, you'll become cool again too*).  You've probably already got it on your system, which is cool.  Its a console based editor that is blazing fast and unfathomably configurable.  Its also the hipster editor of choice these days, so get used to seeing it.  It takes a good long time to learn, but its worth it because its sticking around.  I've been using it for...  20 years?

**Pro-tip**: Don't be tempted by all of the plugin packs or managers.  Put your `~/.vim` and `~/.vimrc` thingies under source control and ease into it.  You'll get lost in no time with plugin packs.  Your choice of plugin management here is deeply personal too but at the end of the day - they all work roughly the same.

## Visual Studio Code

<a href="https://code.visualstudio.com" target="_blank">Visual Studio Code</a> came out of left field as far as editors are concerned.  I had originally figured it would just be a crappy editor focusing on .NET, but whoa was I wrong.  It brings a lot of the niceties of Visual Studio (auto-completion, configurable runners) over to an open source editor and does a damned good job at that.  I find the method of navigating through files a bit odd, but to each their own.
