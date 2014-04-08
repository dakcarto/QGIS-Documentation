.. _contribute_code:

*****************
Contributing Code
*****************

How to Contribute
-----------------

Who can contribute? *Anyone*. Contributions to the core code base are very
welcome.

.. note::

    If you are looking to contribute to the project, but your code needs more
    testing by users or your solution is a little too domain-specific for
    inclusion into the core code base, consider :ref:`crafting and releasing
    your work as a plugin <dev_code>`.

A very high percentage of the QGIS code base has been contributed by
*volunteers*, with the remainder by commercial entities who understand the
importance of contributing their code advancements, or those by developers they
have sponsored, back to the project.

You do not need to be a core developer to commit your crafted code to the QGIS
project. While core developers do have direct write access to the main code
repositories, anyone can submit patches or create git-based pull requests via
github.com.

See: :ref:`managing_source` for information on setting up a local copy of the
QGIS source for your project, and on how to manage your changes in a separate
git branch.

.. _contribute_licensing:

How to License Your Contribution
--------------------------------

QGIS is licensed under the `GPL`_. You should make every effort to ensure
your contributions are unencumbered by conflicting intellectual property rights.
Also, do not submit code that you are not happy to have made available under the
GPL.

.. _GPL: https://www.gnu.org/copyleft/gpl.html

.. _contribute_complete:

How to Make a Complete Contribution
-----------------------------------

Everyone enjoys contributions of new features or bug fixes... until such code
breaks or breaks something else. Complex contributions with the following
qualities, which address completeness, stability and maintenance, will help
ensure the code submission exhibits your due diligence towards sustainability
of the project:

* **Unit test(s)** - Good coding practice includes `unit tests
  <http://en.wikipedia.org/wiki/Unit_testing>`_ to ensure the code continues to
  work as expected. Without a test, there may be no simple means of telling when
  someone breaks your submitted code in the future. Ideally, you should also run
  the full QGIS test suite *prior* to starting your code project, then
  periodically during your development, so you can tell if your code breaks
  existing code.

  Consider adding your unit tests during the coding of your project, when you
  have a firm grasp of the project's problem and solution.

  See: :ref:`debugging_testing` for information on running full QGIS test suite.

* **Python binding(s)** - If applicable, update existing or create new
  `.sip` binding files for your newly added functionality.

  See: :ref:`python_bindings`.

  Adding some Python unit tests allow not only testing of the underlying C++
  code but also its Python binding.

* **Documentation** - Even when your code is highly semantic and very readable,
  it is important to fully document the source code and descriptions of any
  aspect of its public API. This will help users of the generated API
  documentation as well as future developers, including yourself, who may need
  to read the source to understand *why* the code was written in a particular
  manner, especially if the code involves hacks or temporary workarounds.

.. _contribute_pull_request:

How to Contribute via Pull Request
----------------------------------

In general it is easier for developers if you submit GitHub Pull Requests (PR).
We do not describe PRs here, but rather refer you to the GitHub PR
documentation:

https://help.github.com/articles/using-pull-requests

Even if you have fully documented your code, include a complete description for
the pull request. Often core developers, who are also volunteers, are very busy
and a good title and description helps give them a quick overview of your
request.

See: :ref:`pull_requests` for best practices and how to ensure your PR always
stays ready to merge into master branch.

In general when you submit a PR you should take the responsibility to follow it
through to completion - respond to queries posted by other developers, seek out
a 'champion' for your feature and give them a gentle reminder occasionally if
you see that your PR is not being acted on. Please bear in mind that the QGIS
project is driven by volunteer effort and people may not be able to
instantaneously attend to your PR. If you feel the PR is not receiving the
attention it deserves your options to accelerate it should be (*in order of
priority*):

* Send a message to the mailing list 'marketing' your PR and how wonderful it
  will be to have it included in the code base.
* Send a message to the person your PR has been assigned to in the PR queue.
* Send a message to Marco Hugentobler (who manages the PR queue).
* Send a message to the project steering committee asking them to help see your
  PR incorporated into the code base.
