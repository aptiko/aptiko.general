.. _apache:

======
apache
======

Overview
========

This is an Ansible role for installing apache on Debian. It also
optionally installs awstats.

It performs some essential configuration such as enabling several
apache modules, marking 301 redirections as non-cacheable, and changing
the default log format. Other than that, it doesn't do much and should
be combined with :ref:`apache_vhost`.

Parameters
==========

use_ferm
  If ``true``, it drops a configuration snippet in
  :file:`/etc/ferm/ansible-late` in order to allow connections to ports
  80 and 443.  In this case you must also use the :ref:`common` role.
  The default is ``true`` for backwards compatibility reasons.

use_awstats
  If ``true``, it installs and configures awstats. The default is
  ``true`` for backwards compatibility reasons.
