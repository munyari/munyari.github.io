---
layout: post
title: Improve Your Shell Workflow With Aliases
date: 2016-06-14 17:25:50 -0400
categories: programming workflow zsh
---

A shell alias is a name that your shell recognizes as mapped to a command of
your choice. Typically, aliases are shorter than the command that they are
aliased to. A common example is aliasing `g` to `git`.

Aliases are great. Over the long term, they can potentially save a lot of
typing, and they allow you to have an overall more fluid workflow. It seems
pretty intuitive that you'll probably commit to your version control or run
your test suite more often if doing so requires only two or three keystrokes
rather than twenty or thirty. Lowering your barrier to maintaining good habits
can have a big payoff and make you a more effective developer.

You can define an alias in the interactive shell like `alias foo="bar"`, where
`foo` is the alias, and `"bar"` is the command that you are aliasing. This
alias will only be remembered for the duration of the current shell session.
For a more permanent alias, add the alias definition to your shell's startup
script (e.g.  ~/.zshrc, ~/.bash_profile or ~/.bashrc, depending on your setup)
by adding `alias foo="bar"` to the file, where `foo` is the alias and `bar` is
the command that you are aliasing.  After that, you need to either restart your
shell or source the configuration file with `.` or `source`.

```
$ source ~/.bashrc
```

For a shell feature that's supposed to be a convenient time saver, those are a
lot of steps to take everytime you want to add an alias. That means that adding
aliases will probably be a fairly significant disruption to your workflow;
certainly disruptive enough that you'll do so less often than you might
otherwise.

Thankfully, I've written a small shell function that does the heavy lifting for
you.

```bash
al() {
  if [[ $# > 0 ]]; then
    if type $1 > /dev/null; then
      echo "Error: command" $1 "exists" 1>&2
      return 1
    else
      echo alias $1=\"$2\" >> ~/.aliases
    fi
  else
    nvim ~/.aliases
  fi
  source ~/.aliases
}
```

The snippet above defines a shell function call `al()`.  If it is called
without arguments, it opens my text editor to my alias definition file. If the
first argument to `al()` is an existing command, the function will print an
error message and exit with failure (non-zero) status. Otherwise, the new alias
definition is written to my alias file. If the function has not yet returned,
my alias definition file is then sourced by the shell.

A few notes about this snippet: I keep my aliases in a file in my home
directory, ~/.aliases, that I then `source` in my `zshrc` and `bashrc` to keep
my startup scripts fairly short and reduce duplication of common aliases for
both shells. I think it's a useful pattern, but you could just as easily use
your desired startup script. I use [Neovim][neovim] as my text editor, feel
free to swap that out too. Just place this function in your startup script and
restart/source as above - modifying as necessary - to get started.

[neovim]: https://neovim.io/

You can see a list of all the aliases currently defined in your current session
by running `alias` with no arguments. Here are some of the alias definitions
that I find useful.

```
_=sudo
b=bundle
be='bundle exec'
bm='bundle exec rake db:migrate'
brownnoise='play -c 2 -n synth brownnoise'
c=cargo
cb='cargo build'
cr='cargo run'
ct='cargo test'
dbresetseed='bundle exec rake db:migrate:reset && bundle exec rake db:seed'
gu=guard
# yes, I know I shouldn't alias built ins. Fight me.
ls='ls --color --group-directories-first'
n=nvim
pbcopy='xclip -selection clipboard -in'
pbp=pbpaste
pbpaste='xclip -selection clipboard -out'
pinknoise='play -c 2 -n synth pinknoise'
ra=rails
rc='rails console'
rs='rails server'
# common typo
sl=ls
tl='tmux ls'
whitenoise='play -c 2 -n synth whitenoise'
zc='nvim ~/.zshrc'
zr='. ~/.zshrc'
```
