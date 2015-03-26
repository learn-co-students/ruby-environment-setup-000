---
tags: setup, environment, bash
languages: ruby, bash
---

#Environment Setup

You should have already done a bunch of things to set up your environment in the [prework](http://learn.flatironschool.com/lessons/1171), including: installing Yosemite, XCode/Command Line Tools, Homebrew, Git, RVM, Learn gem, and Sublime Text 3.

Now we're going to add a much of other little things to make your development environment even better and customizable.

## Bash Profile

As you've been learning, your bash_profile is a script that runs every time you open or login to your shell. It can configure environment variables, like your `PS1`, which stores your prompt, or `EDITOR`, which is the command other programs will use when they need to launch your default editor.

You can also create aliases for common commands so that they are shorter to use.

And finally, you can also build functions to simplify common workflows.

[Flatiron Bash Profile](https://github.com/flatiron-school/dotfiles/blob/master/bash_profile)

Within that Bash Profile are comments that explain each part. **Make sure to read them!** You can always comment sections in/out to see what they do and how they effect your prompt, shell, and environment. To create this bash_profile locally, simply navigate into your home directory (`cd ~`) and `touch .bash_profile` or if it exists already (`ls -a` and scan the output to see if there is a file .bash_profile - `open .bash_profile`). Copy the raw text from the Flatiron Bash Profile into your bash_profile and save.

Just remember, to activate a change in the dotfile, you must **reload your shell**. You can do that via opening a new tab or typing `source .bash_profile` (from the `~` directory).

## Directory Setup

In order to have an efficient workflow in programming, it's important to have a directory setup that is conducive to navigating multiple directories quickly. Here's how Avi likes to setup directories for development. Some general rules he follows:

- Except for directories in `~`, always use lowercase directory names.
- I prefer `-` to `_` or ` ` to separate words in directory names.
- I want to keep all my code in one place.
- I want to have a place for resources and other things that are related to code, but aren't code.
- I want to be able to get in/out of different code projects easily.

So here's what Avi's filesystem looks like, relevant to code only. Please note that the file directory below is specific to Avi's environment. Your username (`avi` in this case) will vary depending on what your username is.

```
\Users\avi
  \Users\avi\Development
    \Users\avi\Development\books # for books and PDFs and Videos
    \Users\avi\Development\resources # for things like fonts or icons
    \Users\avi\Development\data # for raw SQL or Data Sources I use
    \Users\avi\Development\projects # for things that relate to large projects but aren't code
    \Users\avi\Development\code
      \Users\avi\Development\code\aviflombaum.github.io # Where that is a code project.
      \Users\avi\Development\code\flatiron\sql\homework1.sql # for a specific assignment in flatiron sql unit.
```

Then make use of my `code` function in my `bash_profile` to quickly get into the main code directory where I can easily `cd` into whatever project or `ls | grep` a simple project.

If you want, add the following `lg` function to your bash profile for easy file matching in a current directory.

```
# A function to easily grep for a matching file
# USE: lg filename
function lg {
  FIRST=`echo $1 | sed -e 's/^\(.\).*/\1/'`
  REST=`echo $1 | sed -e 's/^.\(.*\)/\1/'`
  ls -la | grep "[$FIRST]$REST"
}
```

## Git & SQLite3 Updates via Brew

OS X might ship with an old and broken version of git. We want to update it. Try `git version`. Then do `brew install git`. Open a new terminal tab and do `git version` to see if the version was bumped. If it wasn't, try `which git` to see where your command is running from (most likely is `/usr/bin`). The proper way around this is figuring out why `/usr/bin` is appearing first in your `PATH`. Sometimes `brew link git` helps fix that. If not a quick hack is to:

```
sudo mv /usr/bin/git /usr/bin/git.bak
```

That effectively will rename git in `/usr/bin` to a `git.bak` thereby preventing the lookup collision confusion.

The exact same instructions apply to sqlite3. For sqlite3 though you must also do `brew link sqlite3 --force` to create the symlink.

## Sublime Text

We spend most of our time in a text editor. Make sure you have a good one. We use [Sublime Text](http://www.sublimetext.com). Sublime Text 2 and 3 are basically the same, so it's up to you which one you want. Once you have that installed, follow the instructions below.

### Setting up the subl symlink

The first task is to make a symlink to subl. This will allow you to type `subl` in your terminal to launch Sublime Text. Assuming you've placed Sublime Text in the Applications folder, and that you have a `/usr/local/bin` directory in your path, you can run:

#### For Sublime Text 2
```
ln -s "/Applications/Sublime Text 2.app/Contents/SharedSupport/bin/subl" /usr/local/bin
```

#### For Sublime Text 3
```
ln -s "/Applications/Sublime Text.app/Contents/SharedSupport/bin/subl" /usr/local/bin
```

### Install Package Control

If you haven't installed Packages Control, go to [this link](https://sublime.wbond.net/installation).

Sublime Text's functionality can be greatly expanded via plugins called "packages." We won't get into any specific ones to install here, but in order to install them later on down the road, we need to install what is known as Package Control.

Open Sublime Text (you should be able to type `subl` from your terminal now), and then either press:

```
ctrl+`
```

Or, click `View > Show Console`. This will open the Sublime Text console. Then, visit [this link](https://sublime.wbond.net/installation#st2), and copy and paste the code under the Sublime Text 2 tab into the Sublime Text console. Press `enter` and Package Control will be installed.

### Resources
- [RVM, RSpec and Sublime Text 2](http://rubenlaguna.com/wp/2012/12/07/sublime-text-2-rvm-rspec-take-2/)
- [Shortcuts with PDF](http://maxoffsky.com/code-blog/sublime-text-2-shortcuts-printable-format-and-a-gist/)

## Check Up

### gcc

Typing `gcc` should give you something like:

```
clang: error: no input files
```

Not command not found.

### brew

`brew doctor` should reveal no errors. If it tells you that you have macports installed, uninstall it. Other warnings are generally benign at this point.

### terminal

Make sure when you open a new terminal your output looks something like:

```
Last login: Tue Jun  4 22:43:49 on ttys006
[22:44:08] ~
♥
```

If you see some `bash - command not found` type output, it's not a big deal, but let a TA know.

### ruby

`ruby -v` should give you a modern, rvm based ruby, like 2.2.0. `which ruby` should point to an rvm path. Opening a new terminal should maintain ruby versions, if not try `rvm use 2.2.0 --default`

### rvm

`rvm list` should show you installed ruby versions, like 2.2.0.

### sqlite3

`sqlite3 --version` should be >= the 3.7.12 series. If not, do `brew install sqlite3` and `brew link sqlite3 --force` and open a new terminal to retest.

### git

`git --version` should be above 1.8.5. `which git` should point to `/usr/local/bin/git`. If not `brew install git` and `brew link git --force`, new terminal, retest.

### rubygems

`gem env` should output consistent information about your gem env, pointing to similar RVM based paths. Like:

```
RubyGems Environment:
  - RUBYGEMS VERSION: 2.2.1
  - RUBY VERSION: 2.1.0 (2013-06-27 patchlevel 247) [x86_64-darwin13.0.0]
  - INSTALLATION DIRECTORY: /Users/arelenglish/.rvm/gems/ruby-2.1.0
  - RUBY EXECUTABLE: /Users/arelenglish/.rvm/rubies/ruby-2.1.0/bin/ruby
  - EXECUTABLE DIRECTORY: /Users/arelenglish/.rvm/gems/ruby-2.1.0/bin
  - SPEC CACHE DIRECTORY: /Users/arelenglish/.gem/specs
  - RUBYGEMS PLATFORMS:
    - ruby
    - x86_64-darwin-13
  - GEM PATHS:
     - /Users/arelenglish/.rvm/gems/ruby-2.1.0
     - /Users/arelenglish/.rvm/gems/ruby-2.1.0@global
  - GEM CONFIGURATION:
     - :update_sources => true
     - :verbose => true
     - :backtrace => false
     - :bulk_threshold => 1000
  - REMOTE SOURCES:
     - https://rubygems.org/
```

Notice how all the ruby versions correspond in both number and paths?

### Learn Gem

If you have not already installed the learn gem, run `gem install learn-co`. If you get an error saying the gem could not be found, run `gem sources -a http://flatiron:33west26@gems.flatironschool.com` and then try `gem install learn-co` again.

## BASH Extras

### Case-Insensitive Auto-Completion

Take a look in your `.bash_profile`; you should already have this line, which allows your to do case-insensitive tab auto-completion:

```bash
# Case-Insensitive Auto Completion
  bind "set completion-ignore-case on" 
```

## Git Updates

### git branch autocompletion

Autocompletion (pressing tab to autocomplete the name of something) is amazing. You can autocomplete your git branch names through homebrew very easily. Just run:

```
brew install bash-completion
```

Make sure your `.bash_profile` has this somewhere:

```
if [ -f $(brew --prefix)/etc/bash_completion ]; then
  . $(brew --prefix)/etc/bash_completion
fi
```

### .gitconfig

Grab the [Flatiron GitConfig](https://github.com/flatiron-school/dotfiles/blob/master/gitconfig) and create it as `.gitconfig` in your `~`. Also, don't forget to replace the values in some of the person variables like `excludesfile = /Users/<YOUR HOME DIRECTORY>/.gitignore` and:

Pay attention to the cool aliases setup in the git config, like git co for git checkout and git st for git status.

```
[github]
  user = <github username>
  token = <API token> # https://github.com/settings/applications
  email = <github email address>
```

### .gitignore

Read [Github GitIgnore Guide](https://help.github.com/articles/ignoring-files), so you understand what a global gitignore will do and then grab the [Flatiron GitIgnore](https://github.com/flatiron-school/dotfiles/blob/master/gitignore) and create it in `~` as `.gitignore`.

### .gemrc

`gem: --no-ri --no-rdoc`
This will omit installing the rdoc documentation and speed up gem installs.

### .irbrc

```ruby
class Object

  def lm
    (self.methods - Object.methods).sort
  end

  def lim
    (self.instance_methods - Object.instance_methods).sort
  end

end

```
This will allow you to see what methods an object you're working with has but excluding the stuff that exists on all ruby objects.

### base/sqlitebrowser/navicat

Base is an OS X GUI for browsing SQL databases but is not free.
SQLitebrowser is a GUI that only works for SQLITE.
Navicat is a free GUI that works for multiple database types (SQLITE, MYSQL, POSTGRES)
