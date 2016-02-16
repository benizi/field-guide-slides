layout: true

---

class: center, middle, title

# A Field Guide to Software Development

Benjamin R. Haskell

benizi -
[.fa.fa-github[]][github]
[.fa.fa-twitter[]][twitter]
[.fa.fa-facebook[]][facebook]
[.fa.fa-globe[]](http://benizi.com)

[twitter]: https://twitter.com/benizi
[github]: https://github.com/benizi
[facebook]: https://facebook.com/benizi

---

# Disclaimers

- I'm very opinionated about software.
  - It's all terrible.
- I usually think I'm right.
  - Until I see something I think is more correct.
      - Then I'm right again.
- I'm a decent speaker (I think), but bad presenter.
  - Odds are high I'm editing these slides right now.

???

Progression example:
Perl is awesome
=> Perl is read-once
=> Oh, hey, Perl 6 is out.

---

# Disclaimers

- Biases:
  - Used Linux almost exclusively since 1999
--

  - But remember: it's terrible.
  - Some professional Windows experience, but almost all FOSS.
--

- Languages:
  - Professional:
      - Perl => PHP => C# => Ruby => Java/C++
  - Personal:
      - mostly functional
          - Clojure, Haskell, Erlang, Elixir, Rust, OCaml
      - also practical
          - Go, Node.js, Python, VimL, Zsh, Awk, Sed
???

- Perl - young, impressionable
- PHP - ooh, HTML and code together!
- C# - wait, this is ...
- Ruby - ahh, I'm supposed to care about craftsmanship
- Java/C++ - wwwwhhhhhyyyy?
---

layout: false

# Software Development

- What do I mean by "Software Development"?
  - Anything involved in taking an idea for a product and making it a reality.
- Basic steps:
  - Write code
  - Test it
  - Build it
  - Release it
  - Run it
  - Update it
  - Repeat
- [The Twelve-Factor App](http://12factor.net)

???

12 Factor
- set of guiding principles for developing horizontally scalable apps.

Types of scaling have different answers to "We have more traffic. What do we do?"
- Vertically scalable => use a bigger server
- Horizontally scalable => add more servers

---

# Things I'm leaving out

- Planning
  - Only focusing on the nuts and bolts.
  - Hacker at heart. Not an architect.
- Language-specific tools
  - Mainly interested in polyglot problems.

---

# Tools and techniques

- Source Code Management
- Testing
- Problem tracking
- Deployment
- Continuous Integration
- Virtualization

???

These are intertwined.

---

# Source Code Management

- Copies
- RCS (local)
- CVS, Subversion (centralized)
- Git, Mercurial (decentralized)
- GitHub, Bitbucket (centralized decentralized)
- GitLab, Gitolite (self-hosted)

---

# SCM - Copies

- Scientific name: copyus pasteicus
- Distinguishing features:
  - Slightly modified directory or filenames
      - `site` => `new.site.2016-02-15`
      - `site` => `site.for.other.client`
      - `main.pl` => `main.pl.bak`
- Range:
  - Novice developers
  - Stubborn developers
- Conservation status:
  - Everywhere (invasive species)
      - GUI built in to every major O/S
      - <kbd>Ctrl-c</kbd>, <kbd>Ctrl-v</kbd>; <kbd>⌘-c</kbd>, <kbd>⌘-v</kbd>
  - Works on a site that's deployed by FTP

---

# SCM - Copies

- Disadvantages: everything else.
- Mostly a strawman for real SCM.
- Seriously. Don't do this.
--

- Unless you really, really must.
  - closed-source appliances
  - "Edit it live!" on otherwise-SCM-less machine
  - Lots of online commerce "solutions"

---

# SCM - RCS

- Scientific name: Revision Control System
- Distinguishing features:
  - `RCS/filename,v` - versioned file for `filename`
  - Version numbers are `branch.revision`:
      - branch = odd number of numbers, e.g., `1` or `1.1.1`
      - `1.2.3.4` = revision `4` on branch `1.2.3`
  - `RCS` - directory in every directory
- Conservation status: extinct
- Songs and calls:
  - `rcs co file` - check out (default is read-only)
  - `rcs co -l file` - check out (and lock)
  - `rcs ci file` - check in
  - `rcs diff file` - show differences

???

- Unix tradition of saving keystrokes

---

# SCM - RCS

- Adaptations:
  - Diffs are stored as `ed` scripts to get to the prior version

```
# v1.2       # v1.3
pizza        pizza
potatoes     anchovies
```

```
# RCS/ingredients,v (trimmed)

1.3
@
text
@pizza          # <-- text of latest revision
anchovies
@


1.2
*@d2 1           # <-- to get back to 1.2, d(elete, at line )2 1( line)
*a2 1            #                         a(dd, at line )2 1( line)
*potatoes        #                         (contents of line)
@
```

---

# SCM - RCS

- Innovations:
  - Stores (some form of) diffs internally.
  - Log of changes.
      - Including authorship.

```
RCS file: RCS/ingredients,v
Working file: ingredients
head: 1.2
branch:
locks: strict
...
description:
List of ingredients.
----------------------------
revision 1.2
*date: 2016/02/16 03:26:39;  author: bhaskell;  state: Exp;  lines: +1 -0
*Added potatoes.
----------------------------
revision 1.1
*date: 2016/02/16 03:22:55;  author: bhaskell;  state: Exp;
*Initial revision
=============================================================================
```

---

# SCM - CVS

- Scientific name: Concurrent Versions System
  - Basically RCS for multiple files
- Distinguishing features:
  - `CVS` - folder in every subfolder
      - `CVS/Entries` - info about upstream files
      - `CVS/Repository` - info about repo
  - `$CVSROOT` - environment variable to identify set of repositories
  - `Attic` - directory where deleted files go
  - `CVSROOT/modules,v` - where modules are configured
- Songs and calls:
  - `cvs commit` (retains `ci`) - check in changes
  - `cvs checkout` (retains `co`) - check out changes
  - `cvs diff` - find differences
  - `cvs login` - authenticate to a remote server
  - `cvs status` - status of working copy

???

- Weird RCS descended commands

---

# SCM - CVS

- Adaptations:
  - Adds different repository access methods
      - `:pserver:` - CVS-specific
          - Often found near anonymous access
      - `:ext:` - external server method
          - e.g. `ssh` access, then `cvs` invoked locally
  - Authentication
  - Single server serves several modules
  - Editor comment lines:

```
CVS: ----------------------------------------------------------------------
CVS: Enter Log.  Lines beginning with `CVS:' are removed automatically
CVS:
CVS: ----------------------------------------------------------------------
```

---

# SCM - CVS

- Ancestry:
  - Still just RCS for individual file storage

```
$ cvs ci -m 'Added an easy lunch'
cvs commit: Examining .
cvs commit: Examining easy
*RCS file: /my/cvsroot/recipes/easy/lunch,v
done
Checking in easy/lunch;
*/my/cvsroot/recipes/easy/lunch,v  <--  lunch
*initial revision: 1.1
done
```

---

# SCM - SVN

- Scientific name: Subversion
- Distinguishing features:
  - `.svn` directories throughout (historically)
  - Centralized server
  - `trunk`, `branches`, `tags`
      - branches and tags are just conventions
- Songs and calls:
  - "CVS done right"
  - `svnadmin create root`
  - `svn help {command}`

???

- Linus famously(?) derided it:
  - "Because my hatred of CVS has meant that I see
    Subversion as being the most pointless project
ever started, because the whole slogan for the
Subversion for a while was 'CVS done right' or
something like that. And if you start with that
kind of slogan, there is nowhere you can go. It's
like, there is no way to do CVS right." - [Git wiki](https://git.wiki.kernel.org/index.php/LinusTalk200705Transcript)

---

# SCM - SVN

- Innovations:
  - Multiple storage backends
  - Atomic commits
  - First one to have an online ["The &#x5b;X&#x5d; Book"](http://svnbook.red-bean.com/)
  - Server side hooks
      - No check-in without passing tests
      - Enforced styles
  - Properties
      - `svn:executable`
      - `svn:mime-type`
      - `svn:keywords`
- Not dead yet!

???

Atomic commits: CVS is per-file, so can get into broken, inconsistent state

---

# SCM - Git

- Scientific name: Git - the stupid content tracker
- Distinguishing features:
  - Distributed - commits are local, push/pull syncs with other repo
  - Entire history available locally
  - Contains much rope
  - Manuals are terse
      - [git-yield-ref(1) Manual Page][git-yield-ref] (parody!)
      - Assume familiarity with Unix-like systems
  - Simple data model (conceptually)
  - Extensions in any language
- Songs and calls:
  - `git clone https://github.com/user/project` - initial fetch
  - `git push origin master` - update the remote
  - `git rebase --interactive` - rewrite history
  - Commit early, commit often

???

- Git is simple like monads are simple
- Spend years figuring out how to think in Git and it will reward you

- commit early, commit often:
    - It's almost always possible to get back to a state you've committed

[git-yield-ref]: https://git-man-page-generator.lokaltog.net/#6d9d71fca5c14d7d38f61c4edfe6f523
--
- (more after introducing Mercurial)

---

# SCM - HG

- Scientific name: Mercurial
- Distinguishing features:
  - Written in Python
  - Emphasis on speed (hence the name)
- Songs and calls:
  - `hg clone ssh://hg@bitbucket.org/benizi/testhg`
  - `hg pull` - fetch remote revisions

TODO

???

Git's simplicity prevents it from optimizing some
things the way Mercurial does.  e.g. large files

- `hg` = Mercur{y => ial} - get it?

---

# SCM - HG and Git

- Many similarities
  - Decentralized
  - Complete history in every repository
  - Hashes as identifiers
- Competitive with Git in many ways
  - Bigger emphasis on "usability"
      - "friendlier":

```
$ hg ci -m 'Initial commit'
abort: no username supplied
*(use "hg config --edit" to set your username)
```

vs

```
$ git checkout bf90e1dad
...
*You are in a 'detached HEAD' state.
...
```

---

# SCM - HG and Git

- Mercurial
  - Better support for binary files
  - Better support for large repositories
      - Facebook's single repo
  - Better support for shallow clones
- Has kind of "lost", but keeps Git on its toes
  - Made git improve its error messages (slightly)
  - Made git focus on cross-platform (slightly)

---

# SCM - GitHub

- Scientific name: Where software is built
- Songs and calls:
  - Pull Requests (PRs)
  - Issues and wikis
  - Gists - "pastebin" functionality
  - pages - Simple static site hosting

TODO

---

# SCM - Bitbucket

- Scientific name:
- Originally git : GitHub :: hg : Bitbucket
- Now Git is default on Bitbucket, too
- Differentiator:
  - Free private repositories

TODO

[bitbucket]: https://bitbucket.org/

---

# SCM - GitLab

- Scientific name: Code, test, and deploy together

TODO

???

- GitHub = SPOF
  - Host GitHub yourself.

[gitlab]: https://about.gitlab.com/

---

# SCM - Gitolite

- Scientific name:

[gitolite]: http://gitolite.com/gitolite/

TODO - earlier Gitlab-alike
---

# Testing

- Unit testing
- Full-stack testing
- Property testing
- Regression testing
- Smoke testing

---

# Unit testing

- Scientific name: tentatio unit
- Distinguishing features:
  - minimize external dependencies
- Songs and calls:
  - Terminal output: <code>.pass[....].fail[F].pass[....].pending[&#x2A;].pass[....]</code>
  - Must be fast
  - Reduce developer friction

- Given a specific set of inputs, something specific should happen
- Can test functionality, error-handling
- Needs a specification

TODO

???

- Testing "Scientific names" via Google Translate, which ... does Latin
- [minimal testing](https://translate.google.com/#en/la/minimal%20testing)

---

# Full-stack testing

- Scientific name: Vestibulum temptationis
- Distinguishing features:
  - Testing at the domain level
- Songs and calls:
  - Selenium, Webdriver
  - Cucumber: "Given ...", "When ...", "Then ..."
  - Flaky tests

TODO

???

- Google Translate: integrated testing -> porch testing?

---

# Property testing

- Scientific name: probatio rerum
- Related concepts:
  - Generative testing
- Distinguishing features:
  - Computers are better at randomly-generating things
- Range:
  - Mostly functional languages
- Subspecies:
  - QuickCheck

???

- Google Translate: property testing => "~ things trials"

- One of my favorite techniques that isn't widespread.

---

# Regression testing

- Scientific name:

---

# Smoke testing

- Scientific name:

---

# Problem tracking

- Issue tracking
  - Internal, future, Business
- Exception tracking
  - External, past, DevOps

---

# Issue tracking

- Olden times
  - Bugzilla
  - Trac
- Web 2.0
  - Trello
  - Pivotal Tracker
- JIRA
- GitHub

---

# Exception tracking

- Sentry
- Bugsnag
- Honeybadger
- Airbrake

---

# Integration

- With other services
  - Webhooks
  - JSON APIs
- Continuous integration

---

# Integration with other services

- Webhooks
  - HTTP + JSON
    - When an event occurs on Service A, POST a small JSON payload to Service B
    - Usually has a configuration interface
- JSON APIs
  - More possibilities than Webhooks

---

# Chat Ops

- HipChat
- Slack
- IRC

---

# Continuous Integration

- Jenkins
- Travis
- Codeship
- CircleCI

---


# Jenkins

- Scientific name:
- Range:
  - Originally Java projects
  - Now everywhere

TODO

---

# Travis

- Scientific name:

---

# Other CI

- Codeship
- CircleCI
TODO

---

# Virtualization

- Virtual Machines
- Containers
- Docker

---

# Virtual Machines

- Contains a whole O/S
- Major providers:
  - VMware
  - VirtualBox
  - QEMU
- Range:
  - VMware - Windows, security, data centers
  - VirtualBox - FOSS

TODO

---

# Containers

- Shares the kernel with the host
- Isolation through namespaces (Linux), zones (Solaris), jails (BSDen)

TODO

---

# Docker

- Standardized format for container deployment

TODO

---

# Example App

- Written in [Elixir][elixir] using [Phoenix Framework][phoenix]
- GitHub for SCM [benizi/field-guide-app][github-app]
- Honeybadger for exception tracking
- Travis CI
- Heroku for hosting
- Slack integration
- 

TODO

???

[elixir]: http://elixir-lang.org/
[phoenix]: http://www.phoenixframework.org/
[github-app]: https://github.com/benizi/field-guide-app

---

# Resources

- [Stackshare.io](http://stackshare.io)
  - Great for finding categorized tools
  - Most mentioned here are in the [DevOps category](http://stackshare.io/devops)
- Similar: [alternativeTo.net](http://alternativeto.net), e.g. [Jenkins](http://alternativeto.net/software/jenkins/?platform=linux)
