Description
===========

Ocean is a general purpose library, compatible with both D1 and D2, with a focus
on supporting the development of high-performance, real-time applications. This
focus has led to several noteworthy design choices:

* **Ocean is not cross-platform.** The only supported platform is Linux.
* **Ocean assumes a single-threaded environment.** Fiber-based multi-tasking is
  favoured, internally.
* **Ocean aims to minimise use of the D garbage collector.** GC collect cycles
  can be very disruptive to real-time applications, so Ocean favours a model of
  allocating resources once then reusing them, wherever possible.

Ocean began life as an extension of `Tango
<http://www.dsource.org/projects/tango>`_, some elements of which were
eventually merged into Ocean.


Preview Release
===============

As an important note, we want to emphasize this is a *preview* release, in the
sense that the main development of this library will still be conducted
internally at Sociomantic in a private repository, and we'll only synchronize
it when we do new releases. This is because we are still lacking the needed
infrastructure to do all the development in the wild (most notably automatic
testing).

We want to switch to a completely public development model, but when this will
happen is still uncertain (although we hope soon).

That said, we welcome pull requests and issues, we'll see how to deal with this
duality when the time comes, but we are definitely willing to accept
contributions.


Build / Use
===========

Dependencies
------------

This library has quite a few number of dependencies, but it depends on which
modules you want to use. Usually the easiest way to find out is just using it
and see which libraries the linker fails to find and then install by demand.

If you want to install everything, then the list is as follows (for an
absolutely up to date list you can take a look at the ``Build.mak`` file, in
the ``$O/%unittests`` target):

* ``-lglib-2.0``
* ``-lpcre``
* ``-lxml2``
* ``-lxslt``
* ``-lebtree``
* ``-lreadline``
* ``-lhistory``
* ``-llzo2``
* ``-lbz2``
* ``-lz``
* ``-ldl``
* ``-lgcrypt``
* ``-lgpg-error``
* ``-lrt``

Please note that ``ebtree`` is not the vanilla upstream version. We created our
own fork of it to be able to write D bindings more easily. You can find the
needed ebtree library in https://github.com/sociomantic-tsunami/ebtree/releases
(look only for the ``v6.0.socioX`` releases, some pre-built Ubuntu packages are
provided).

If you plan to use the provided ``Makefile`` (you need it to convert code to
D2, or to run the tests), you need to also checkout the submodules with ``git
submodule update --init``. This will fetch the `Makd
<https://github.com/sociomantic-tsunami/makd>`_ project in ``submodules/makd``.


Conversion to D2
----------------

Once you have all the dependencies installed, you need to convert the code to
D2 (if you want to use it in D2). For this you also need to build/install the
`d1to2fix <https://github.com/sociomantic-tsunami/d1to2fix>`_ tool.

Also, make sure you have the Makd submodule properly updated (see the previous
section for instructions), then just type::

  make d2conv


D2 Compatibility
----------------

The resulting code should work at least in upstream vanilla DMD v2.70.x, with
the exception of a few modules that depends on the old Tango runtime function
``gc_usage()``. There is a dummy implementation returning all zeros just to at
least allow compiling stuff, but until `#1591
<https://github.com/dlang/druntime/pull/1591>`_ is merged into druntime, these
functions will remain slighty broken, and the tests (``make DVER=2`` after
``make d2conv``) will fail too.



Support Guarantees
------------------

* Major branch development period: 6 months
* Maintained minor versions: 2 most recent

Maintained Major Branches
-------------------------

====== ==================== ===============
Major  Initial release date Supported until
====== ==================== ===============
v2.x.x v2.0.0_: 30/06/2016  TBD
====== ==================== ===============
.. _v2.0.0: https://github.com/sociomantic-tsunami/ocean/releases/tag/v2.0.0

Releases
========

`Latest release notes
<https://github.com/sociomantic-tsunami/ocean/releases/latest>`_ | `All
releases <https://github.com/sociomantic-tsunami/ocean/releases>`_

Ocean's release process is based on `SemVer
<https://github.com/sociomantic-tsunami/ocean/blob/v2.x.x/VERSIONING.rst>`_. This means
that the major version is increased for breaking changes, the minor version is
increased for feature releases, and the patch version is increased for bug fixes
that don't cause breaking changes.

Releases are handled using GitHub releases. The notes associated with a
major or minor github release are designed to help developers to migrate from
one version to another. The changes listed are the steps you need to take to
move from the previous version to the one listed.

The release notes are structured in 3 sections, a **Migration Instructions**,
which are the mandatory steps that users have to do to update to a new version,
**Deprecated** which contains deprecated functions that are recommended not to
use but will not break any old code, and the **New Features** which are optional
new features available in the new version that users might find interesting.
Using them is optional, but encouraged.
