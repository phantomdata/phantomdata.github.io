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
