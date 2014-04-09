.. _managing_source:

***************
Managing Source
***************

.. contents::
   :local:
   :backlinks: top

.. _git_install:

Installing Git
==============

This section describes how to get started using the QGIS git repository. Before
you can do this, you need to first have a git client installed on your system.

Install Git for GNU/Linux
-------------------------

Debian based distro users can do::

    $ sudo apt-get install git

Install Git for Windows
-----------------------

Windows users can obtain `msys git <http://code.google.com/p/msysgit>`_ or use
git distributed with `cygwin <http://cygwin.com>`_.

Install Git for Mac OS X
------------------------

The `git <http://git-scm.com/>`_ project has a downloadable build of git. Make
sure to get the package matching your processor (x86_64 most likely, only the
first Intel Macs need the i386 package).

Once downloaded open the disk image and run the installer.

PPC Note
........

The git site does not offer PPC builds.  If you need a PPC build, or you just
want a little more control over the installation, you need to compile it
yourself.

Download the source, unzip it, and in a Terminal ``cd`` to
the source folder, then::

  $ make prefix=/usr/local
  $ sudo make prefix=/usr/local install

If you don't need any of the extras, Perl, Python or TclTk (GUI), you can
disable them before running make with::

  $ export NO_PERL=
  $ export NO_TCLTK=
  $ export NO_PYTHON=

.. _git_documentation:

Git Documentation
=================

See the following sites for information on becoming a git master.

http://git-scm.com/documentation
http://gitref.org
http://progit.org
http://gitready.com

GUI clients for git:

http://git-scm.com/downloads/guis

.. _git_getting_source:

Getting QGIS Source
===================

All of the QGIS repositories are located at:

https://github.com/qgis

The simplest means of access to the source for QGIS desktop is to fork its
repository to your own github.com account. Then, clone it from your account to
your local computer.

See: `github forking help <https://help.github.com/articles/fork-a-repo>`_

Example::

    $ git clone git@github.com:<github_user>/QGIS.git

To clone *just* master branch from your forked QGIS desktop repository, but no
other branches::

    $ mkdir <qgis-src-dir> && cd <qgis-src-dir>
    $ git init
    $ git remote add -t master -f origin git@github.com:<github_user>/QGIS.git

Once you have cloned your QGIS source from your own github account, add the
'qgis' account's repository as a second git remote::

    $ git remote add upstream https://github.com/qgis/GIS.git

.. note::

    If you have write access to upstream use::

        $ git remote add upstream git@github.com:qgis/QGIS.git

This allows you to pull in changes from the official 'upstream' QGIS repository
to your local git branches. There is no simple way to *update* your github fork
with changes from upstream, without using an intermediate git clone. Using the
extra upstream remote to pull in changes to your locally-cloned repository, then
pushing to your github account is one way you to keep your github account
up-to-date with QGIS upstream changes.

.. _qgis_documentation_source:

Getting QGIS Documentation Sources
==================================

If you're interested in checking out QGIS documentation sources::

    $ git clone https://github.com/qgis/QGIS-Documentation.git

.. note::

    If you have write access to upstream use::

        $ git clone git@github.com:qgis/QGIS-Documentation.git

You can also take a look at the README included with the documentation
repository for more information.

.. _git_checkout:

Checking Out a Branch
=====================

To check out a branch, for example the release 1.7.0 branch, do::

    $ cd <qgis-src-dir>
    $ git fetch
    $ git branch --track origin release-1_7_0
    $ git checkout release-1_7_0

To check out the master branch::

    $ cd <qgis-src-dir>
    $ git checkout master

.. note::

    In QGIS we keep our most stable code in the current release branch. Master
    contains code for the so called 'development' or 'testing' series.
    Periodically we will branch a release off master, and then continue
    stabilization and selective incorporation of new features into master.

See :ref:`install_qgis` for specific instructions on building development
versions.

.. _git_work_in_branches:

Working in Branches
===================

Purpose
-------

The complexity of the QGIS source code has increased considerably during the
last years. Therefore it is hard to anticipate the side effects that the
addition of a feature will have. In the past, the QGIS project had very long
release cycles because it was a lot of work to reetablish the stability of the
software system after new features were added. To overcome these problems, QGIS
switched to a development model where new features are coded in git branches
first and merged to master (the main branch) when they are finished and stable.
This section describes the procedure for branching and merging in the QGIS
project.

Procedure
---------

**Initial announcement on mailing list**:

Before starting, make an announcement on the developer mailing list to see if
another developer is already working on the same feature. Also contact the
technical advisor of the Project Steering Committee (PSC). If the new feature
requires any changes to the QGIS architecture, a request for comment (RFC) is
needed.

**Create a branch**:

Create a new git branch for the development of the new feature::

  $ git branch newfeature
  $ git checkout newfeature

Now you can start developing.

Example Git Workflow
--------------------

In the following examples an additional remote named 'upstream' that references
the official upstream QGIS repository is assumed.

The first thing we need to do is pull down the latest changes from the main QGIS
repository::

    $ cd <qgis-src-dir>
    $ git fetch upstream
    remote: Counting objects: 13, done.
    remote: Compressing objects: 100% (1/1), done.
    remote: Total 7 (delta 6), reused 7 (delta 6)
    Unpacking objects: 100% (7/7), done.
    From github.com:qgis/Quantum-GIS
       18cd145..89bdb10  master     -> upstream/master

Now that we have the changes in our local repository we need to bring our master
branch up to date with the latest changes from upstream. Use ``rebase`` here
because we don't want to see many extra merge commits, for each time we want to
bring out local master branch up to date. In the end we want the local master
branch to reflect upstream/master exactly::

    $ git rebase upstream/master
    First, rewinding head to replay your work on top of it...
    Fast-forwarded master to upstream/master.

.. note::

    You can combine the two into one call using::

        $ git pull upstream master --rebase

In order to do any work in git you should really be using separate branches,
instead of working on master, i.e keep it a clean mirror of the upstream master
branch. We can check out a new one 'working' or 'feature' branch using::

    $ git checkout -b working
    Switched to a new branch 'working'

This will checkout a new working branch off our local master branch and switch
to it.

Let's do some work::

    $ git commit -a -m "Add some feature"
    [working 8cd2f4b] Add some feature

    $ git commit -a -m "More feature stuff"
    [working 72d30ad] More feature stuff

    $ git commit -a -m "bug fix"
    [working 25b10e5] bug fix

    $ git commit -a -m "bug fix"
    [working 211e387] bug fix

.. note:: The -a means add any changed files to the commit. You can also use
   ``git add``.

.. _git_merge_rebase:

Merging and Rebasing
....................

Now at this point we could merge our changes into the master branch and push it
up, or if you don't have commit rights you can issue a pull request. However
having heaps of "fix this" and "fix that" commits is pretty ugly. This is where
``git rebase`` can come in handy.

We can check which commits we have added that are not in master by doing::

    $ git log --oneline master..
    211e387 bug fix
    25b10e5 bug fix
    72d30ad More feature stuff
    8cd2f4b Add some feature

.. figure:: /static/developer_guide/managing_source/git2.png
   :align: center
   :width: 50em

There we can see we have four commits that differ and that 8cd2f4b is the first
commit we made. We really want to merge all the commits into one to make this a
little cleaner::

    $ git rebase -i 8cd2f4b^

.. note::

   ^ means go back one commit from the one listed. ``git rebase`` doesn't
   include the commit that you list so you have to go back one before it. You
   can also use ``git rebase -i HEAD~n`` to rebase *n* number of commits back
   from HEAD.

This will show up in your defined git ``core.editor``::

    pick 8cd2f4b Add some feature
    f 72d30ad More feature stuff
    f 25b10e5 bug fix
    f 211e387 bug fix

    # Rebase 89bdb10..7d02daf onto 89bdb10
    #
    # Commands:
    #  p, pick = use commit
    #  r, reword = use commit, but edit the commit message
    #  e, edit = use commit, but stop for amending
    #  s, squash = use commit, but meld into previous commit
    #  f, fixup = like "squash", but discard this commit's log message
    #  x, exec = run command (the rest of the line) using shell
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    # However, if you remove everything, the rebase will be aborted.

We have changed all but the first commit to **f** this will merge all the
commits into the first one. The latest commit is at the bottom so you should
read the rebase screen from bottom up

Result::

    [detached HEAD d5620a5] Add some feature
     1 file changed, 3 insertions(+)
     create mode 100644 test.txt
    Successfully rebased and updated refs/heads/working.

.. figure:: /static/developer_guide/managing_source/git1.png
   :align: center
   :width: 50em

At this point we normally merge it into master and push it upstream, but if you
don't have commit rights then you can push it up to your github repository and
open a pull request::

    # Push them up for review (assuming origin = your github.com account remote)
    $ git push origin working

.. warning::

    ``git rebase -i`` will change the commit hash for anything that is included
    in the range of commits. Make sure you only rebase commits that are *NOT*
    public yet. **Only rebase commits in your local repository that do not yet
    exist on the remote branch.**

    If you rebase existing public commits and have write access to the main QGIS
    repository, your next commit to upstream will screw up the main repository.
    **CORE DEVS: DO NOT DO THIS!**

.. note::

    Most of the above content in the previous two sections was originally
    published in `Nathan Woodrow's blog
    <http://nathanw.net/2013/02/05/my-qgis-git-workflow/>`_.

.. _pull_requests:

Managing Pull Requests
======================

When you are ready to submit your work as a pull request (PR) to the main
upstream QGIS repository, first review :ref:`contribute_code`, including
:ref:`contribute_pull_request`, to make sure you have covered the basics of
crafting your committed code to be *sustainable*.

.. note::

   If your code is incomplete, and you need to bounce the idea off of some
   fellow developers, you can still submit a PR, just be sure to indicate this
   in its description. A commonly used term in this regard is 'WIP' or
   work-in-progress.

During the course of working on the PR, you can add commits to its branch and
they will automatically show up in the PR. When the testing of the PR is
complete and the code ready to commit, you can still clean up the branch using
the *rebase* method noted above. However, in order to update the remote branch
you will have to *force push* to it::

    $ git push origin pr_branch -f

.. note::

   This will put your repository's github branch out-of-sync with its previous
   state, i.e. will screw up any previously checked-out copy of your branch,
   like those checked out by devs testing your origin PR branch. Anyone using
   the previous branch will need to delete it and checkout the new rebased one.

If your PR is merged into master branch, you can then safely remove the PR’s
branch from your fork.

Core Developer Merging
----------------------

**Option A**

* Click the Merge button (creates a non-fast-forward merge, adding "merge
  commit" to history)

**Option B**

#. Checkout the pull request
   (see https://gist.github.com/piscisaureus/3342247 )
#. Test (also required for Option A, obviously)
#. Checkout master, ``git merge pr/1234``
#. Optional: ``git pull --rebase`` which creates a fast-forward, no "merge
   commit" is made (cleaner history, but it is harder to revert the merge)
#. ``git push``

.. _generating_patches:

Generating Patches
==================

Pull requests are the recommended means of contributing, but patch files are
still useful for quickly conveying some small edits, e.g. when working on issues
in the tracker.

Patch File Naming
-----------------

If the patch is a fix for a specific bug, please name the file with the bug
number in it e.g.  bug777fix.patch, and attach it to the original bug report
in `the tracker <http://hub.qgis.org/projects/quantum-gis>`_.

If the bug is an enhancement or new feature, it's usually a good idea to create
a ticket in `the tracker <http://hub.qgis.org/projects/quantum-gis>`_ first,
then attach your patch to it.

Creating your patch
-------------------

Create your patch in the top level QGIS source directory.

This makes it easier for us to apply the patches since we don't need to navigate
to a specific place in the source tree to apply the patch. Also when we receive
patches we usually evaluate them using merge, and having the patch from the top
level directory makes this much easier. Below is an example of how you can
include multiple changed files into your patch from the top level directory::

  cd <qgis-src-dir>
  git checkout master
  git pull origin master
  git checkout newfeature
  git format-patch master --stdout > bug777fix.patch

This will make sure your master branch is in sync with the upstream repository,
and then generate a patch which contains the delta between your 'newfeature'
branch and what is in the master branch.

Getting your patch noticed
--------------------------

QGIS developers are busy folk. We do scan the incoming patches on bug reports
but sometimes we miss things. Don't be offended or alarmed. Try to identify a
developer to help you - using the `Technical Resources
<http://qgis.org/en/site/getinvolved/governance/organisation/governance.html#community-resources>`_
and contact them, asking if they can look at your patch. If you don't get any
response, you can escalate your query to one of the Project Steering Committee
members (contact details also available in the Technical Resources).

Obtaining Core Write Access
===========================

Write access to QGIS source tree is by invitation. Typically when a person
submits several (there is no fixed number here) substantial patches that
demonstrate basic competence and understanding of C++ and QGIS coding
conventions, one of the PSC members or other existing developers can nominate
that person to the PSC for granting of write access. The nominator should give a
basic promotional paragraph of why they think that person should gain write
access. In some cases we will grant write access to non C++ developers e.g. for
translators and documentors.  In these cases, the person should still have
demonstrated ability to submit patches and should ideally have submitted several
substantial patches that demonstrate their understanding of modifying the code
base without breaking things, etc.

Note: Since moving to git, we are less likely to grant write access to new
developers since it is trivial to share code within github by forking QGIS and
then issuing pull requests.

Always check that everything compiles before making any commits / pull requests.
Try to be aware of possible breakages your commits may cause for people building
on other platforms and with older / newer versions of libraries.

When making a commit, your editor (as defined in $EDITOR environment variable)
will appear and you should make a comment at the top of the file (above the area
that says 'don't change this'. Put a descriptive comment and rather do several
small commits if the changes across a number of files are unrelated. Conversely
we prefer you to group related changes into a single commit.
