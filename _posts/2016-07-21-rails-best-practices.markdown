---
title: phantomdata's Rails Best Practices
author: Jordan T. Norlen
date: 2016/07/21
summary: A collection of Rails Best Practices and lessons hard learned over many years of doing Rails development
layout: post
---

# Why Another Guide?

> The quickest way to permanent is temporary

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

# Editor

How you run your actual minute to minute interactions with any discipline (except .NET) is a deeply personal choice.  Do you like light-weight editors with no frills?  Perhaps instead you prefer looming masses of scurrying robots in an IDE?  The choice is yours.  Its one that I highly encourage you to experiment with until you find just what is right for you.

That being said, I've outlined a few of the *common* choices below in alphabetical order.  I'm kind of an editor-junky and have about 25 different editors on my system right now, so don't think I haven't tried your favorite.  :)

## Atom

<a href="https://atom.io/" target="_blank">Atom</a> is a relative new-comer to the scene.  It's a semi-frills editor based around JavaScript.  Consequently, it can be kind of slow.  There are however a ton of plugins which can greatly aid your experience with Rails.  Once you've installed it, be sure to check some out.

## RubyMine

<a href="https://www.jetbrains.com/ruby/" target="_blank">RubyMine</a> is the hulking behemoth in the room.  I actually reach for this every time I'm opening a brand new code base to help me get my bearings.  Its got a ton of features and is absolutely awesome at helping you explore what you're working on.  This comes at a price though.  Its expensive, for one, but its also incredibly taxing on your CPU.  I certainly never run this when I have a MacBook on my lap.

## Sublime

<a href="https://www.sublimetext.com/3" target="_blank">Sublime</a> is the perpetual beta darling.  I'm fairly certain that they're never going to release an official 'stable' build of this editor.  Which is fine, because its rock solid.  Its a GUI based IDE that's easy to customize and navigate within.  There's not a whole lot going on, but it is *fast*, *stable* and *easy*.

## textmate

<a href="https://macromates.com/download" target="_blank">textmate</a> is the original Rails editor.  Its actually what made me switch over to OSX way back before I could grow a beard and had a bald spot.  Its a stable and fast editor that gave birth to the current crop.  Its biggest feature (imho) was command-p, which allowed you to simply hit command-p and type the name of the file you wanted.  Splendid.

An interesting note, although textmate seems to have had its development stopped and is no longer seen *anywhere* - it seems to be what DHH is still using.  I just spied it on the recent Rails 5 announcement video and was floored.  Dedication.

## Vim

<a href="http://www.vim.org/" target="_blank">Vim</a> is the grand-daddy of editors (*sssssh emacs, you'll become cool again too*).  You've probably already got it on your system, which is cool.  Its a console based editor that is blazing fast and unfathomably configurable.  Its also the hipster editor of choice these days, so get used to seeing it.  It takes a good long time to learn, but its worth it because its sticking around.  I've been using it for...  20 years?

**Pro-tip**: Don't be tempted by all of the plugin packs or managers.  Put your `~/.vim` and `~/.vimrc` thingies under source control and ease into it.  You'll get lost in no time with plugin packs.  Your choice of plugin management here is deeply personal too but at the end of the day - they all work roughly the same.

## Visual Studio Code

<a href="https://code.visualstudio.com" target="_blank">Visual Studio Code</a> came out of left field as far as editors are concerned.  I had originally figured it would just be a crappy editor focusing on .NET, but whoa was I wrong.  It brings a lot of the niceties of Visual Studio (auto-completion, configurable runners) over to an open source editor and does a damned good job at that.  I find the method of navigating through files a bit odd, but to each their own.

# Setting Up Your Project

OK, so you've probably seen (and maybe even done) some of the "Get Started in 5 minutes!" blog posts.  That's fantastic, I mean it.  You've gotten a taste for what Rails is about.  What those articles don't give you, though, is a firm foundation to start a *quality* software project.  Something that will stand the test of time and other dirty little fingers poking about in your code.

There's some actual work here beyond `rails new MyProject`.

**Be Warned** - what follows are my *opinions*.  There are forty-five-thousand right ways to tackle these issues and most people will argue that theirs is the one true way.  My advice is to pick one, try it and see how it feels for you before trying another.

## Where to Store Your Code

Even before you start poking around, you need to figure out where you're going to store your code.  *No*, your local disk isn't appropriate.  Think about just how angry you'd be at your cat if it knocked over your computer after even spending only 8 hours on your project.  In today's world, there's plenty of places to store your code online - so you really have no excuse.

Might I recommend <a href="https://github.com" target="_blank">GitHub</a>?  They provide free source code hosting for open source projects and you can integrate with *tons* of free services to help you maintain code quality.

*A word of caution*: Potential employers will likely look at your GitHub code.  Since you're reading this document, you're probably interested in becoming a code craftsman - but be careful about putting play-around code up there.  If you are currently engaged in a job search, you might consider <a href="https://bitbucket.com" target="_blank">BitBucket</a> instead.  It provides a far less appealing interface and integrates with little, but it provides free private repositories.

A recent contender in this market is <a href="http://gitlab.com">GitLab</a>.  Their free version is fairly slow, but does allow for many more features than what GitHub provides.  Its worth checking out.

### Branching Strategies

Once you've got your code hosted somewhere, you'll need to develop a branching strategy.  Sure, you could just pop all your code into master and call it good - but you're in for a world of hurt down the line.  A good branching strategy allows you and your team to make changes with the confidence that you won't break other ongoing work - or accidentally release your whiz-bang half baked unfinished feature.  :)

A simple branching strategy (that should go into your README.md) might look something like this:

1. There are 2 primary branches: `master` and `develop`
1. All work is done off of a feature branch from develop (`git checkout -b feature/cat-text-everywhere`)
1. When that work is complete, create a pull request to have it merged into develop
1. Someone will review that pull request and offer feedback
1. Once develop is stable and features for a given milestone are complete, develop is merged into master
1. Continuous Deployment systems (described later) will take over and push the code out to the servers
1. Hotfixes or emergency maintenance will be done in a branch off of `master` that is merged with a PR and then pushed out by the CD server

In essence, if you want to make a change; `git checkout -b feature/my-change` off of develop.  Issue a PR to get it back into develop.  Once hardened, that goes to master and then out to the production servers.

## Testing Libraries

I have no idea why, but the community has decided to back *rspec* for testing under Rails.  Rails has insisted on shipping with Minitest for several versions, but *rspec* is where its at.  Don't even be tempted to try using the built-in testing suite, you will fail at all of your google searches and be thwarted at every turn.  Just add `gem 'rspec-rails'` to your Gemfile and move along.

On the topic of exactly how to test with rspec, <a href="https://robots.thoughtbot.com/how-we-test-rails-applications" target="_blank">thoughtbot</a> has an excellent overview article on the generalities of testing with rspec that I would also suggest you read and consider.  Rspec also has <a href="http://rspec.info/documentation/" target="_blank">some great documentation</a> that you can peruse.

Once you've got a basic handle on what it means to use rspec, don't forget about <a href="http://www.rubydoc.info/gems/factory_girl/file/GETTING_STARTED.md" target="_blank">Factory Girl</a>.  Factory Girl provides a clean and simple way to create objects for you to use while testing.  After awhile of not using it, and after having written your 15th "create_user_with_pictures_of_cats" method - you'll understand its purpose.

## Continuous Integration

CI is really just a thingie that runs your tests and bitches at you.  Its like having your Mom constantly check to see if you've cleaned your room.  Ordinarily, that sort of thing is pretty annoying.  When you're building a real software project with the potential for multiple people, however, its essential.  CI - in conjunction with your tests - helps ensure that your *users* are never the ones to complain about your messy room.

The answer here in Rails is pretty simple; just use <a href="https://circleci.com/" target="_blank">CircleCI</a>.  Its generally free for open source projects and offers a wealth of options.  It can even act as a Continuous Deployment gateway - which I'll get to in a moment.

## Hosting

*Waaaaaah!  I haven't even started coding yet!*, I hear you screaming.  I know.  I know.  Trust me, this will all save you a world of hurt later on.  Setting up your hosting now will prevent a ton of headaches down the line once your application is finished.

There are occasionally some fundamental architectural changes that will need to be made to meet hosting needs and its better to find them out before you've got code up and running.  Finally, once you've put that last little dollop of cream on your shiny new application - the worst thing in the world is to spend 4 hours configuring a hosting provider when all you wanted to do was show it to your friend.

Ok, so there's a huge can of worms here.  In my book, you've got two choices.  <a href="http://heroku.com" target="_blank">Heroku</a> and <a href="http://aws.amazon.com">AWS</a>.  One is a beautiful butler served dinner on silver platters with classical music in the background; and the other is a burning farm with horses running wild and cats all over the place.  They each have their place though.

### Heroku
Heroku is a hosting as a service platform.  For hobby projects and start-ups, its a great place.  You can get your code up and running in a matter of clicks with no issue.  Its interface is beautiful, its tooling easy to use and its a joy to work with.  The down-side is that its expensive.  Like, really expensive.  Like, thousands of dollars a month in hosting a real-application expensive.

Now, its free for hobby projects - which is great for us tinkerers.  Its also fairly reasonable for low to medium traffic business applications.  Its made particularly reasonable by the ease of setup and maintenance - there's basically none there.  For early stage applications, this can often times be way cheaper than paying a DevOps engineer $150k in an annual salary.

### AWS

AWS on the other hand, is really cheap.  I can spin up and configure a fully hosted medium-traffic site for around $15/month all said and done.  Not bad.  Where it hits you is in the time that you need to put in to learning and managing it.  AWS has a ton of features that can help you, but that comes at the cost of needing to know how to use them.  You'll need to learn some DevOps chops to fully leverage it - which can command a hefty salary - but in the end you'll have infinite scalability and a tolerable hosting bill.

### Verdict

Use Heroku for hobby and start-up-style applications; use AWS for bigger business applications that you see reaching large numbers of users in short order.

## Continuous Deployment

This is a cool one.  Historically, Rails apps have been deployed with **Capistrano**.  That was fantastic in the days where we were taming the wild-west of PHP coders who just FTP'ed files to a shared host.  It enforced structure and maintained a high degree of quality in a world where there was none.

Those days have passed though, and we're looking for something new.  With the relatively recent standard of implementing CI and a high emphasis on quality code, we can have higher confidence in the code-bases that are living in our SCM.  This confidence grants us the ability to deploy directly from our code when it is deemed "stable" by a computer - not a human.

In practice, this means that we can setup systems like *CircleCI* to automatically deploy our code out from `master` whenever a passing build is encountered.  Freeing us from the arduous task of being up at 3AM to run a risky deployment script that may or may not break things.

Once you've gotten the above components in place, you're probably looking at a GitHub + CircleCI + Heroku configuation.  At this point, you can simply follow CircleCI's <a href="https://circleci.com/docs/continuous-deployment-with-heroku/" target="_blank">documentation</a> on deploying to Heroku.  Note that you will want to use the section entitled "Heroku with pre- or post-deployment steps" because...  what application doesn't have a database that needs to be migrated?
