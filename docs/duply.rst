=====
duply
=====

Overview
========

This is an Ansible role for making a server backup itself using the
duply_ front-end to duplicity_.  It installs a duply profile called
"main".

This role does not currently support encryption.

.. _duply: http://duply.net
.. _duplicity: http://duplicity.nongnu.org/

What is backed up
=================

Everything is backed up, except for stuff that is reproduced during
installation (such as :file:`/usr`), can be recreated (such as
:file:`/var/cache`), is temporary (such as :file:`/tmp` and :file:`/var/tmp`), or
should be backed up in another way (such as live database files).

More specifically, the files and directories excluded are :file:`/bin`,
:file:`/boot`, :file:`/dev`, :file:`/initrd.img`, :file:`/lib`,
:file:`/lib32`, :file:`/lib64`, :file:`/lost+found`, :file:`/media`,
:file:`/mnt`, :file:`/opt`, :file:`/proc`, :file:`/run`, :file:`/sbin`,
:file:`/sys`, :file:`/tmp`, :file:`/usr`, :file:`/var/cache`,
:file:`/var/crash`, :file:`/var/lib/postgres`, :file:`/var/lib/mysql`,
:file:`/var/lock`, :file:`/var/run`, :file:`/var/swap`,
:file:`/var/swapfile`, :file:`/var/swap.img`, :file:`/var/tmp`, and
:file:`/vmlinuz`. There is currently no option for changing that.

Pre- and post- scripts
======================

The :file:`/etc/duply/main/pre-scripts` and
:file:`/etc/duplicity/post-scripts` directories contain scripts that
will be executed before and after backup. Each ansible role that needs
this should drop executable files in these directories. These files are
executed in no particular order.  :file:`/etc/duply/main/pre` is
particularly useful for storing database dumps.

Since ``duply`` does not directly support having many scripts, but only
a single pre and a single post script (in :file:`/etc/duply/main/pre`
and :file:`/etc/duply/main/post`), this is implemented by creating a
``pre`` and a ``post`` script that merely run scripts in these
directories. Don't touch :file:`/etc/duply/main/pre` and
:file:`/etc/duply/main/post` themselves.
  
When backup occurs
==================

The backup script is just thrown in :file:`/etc/cron.daily/duply`, so
backup occurs when the OS is configured to run daily cron scripts; by
default 06:25. There is currently no option available to change this.

Parameters
==========

duply_deactivate
  Default ``false``. Set to ``true`` in order to deactivate backup of a
  server.  This may be useful when setting up backup servers themselves.

duply_target
  No default.

duply_verbosity
  Default "notice". See also the ``duply_max_fulls_with_incrs``
  parameter below.

duply_max_fullbkp_age
  How often to do full backups; default 3M.

duply_max_age
  Oldest backup to keep.  Default 2Y.

duply_max_fulls_with_incrs
  Delete incremental sets of all backup sets that are older than the
  count:th last full backup. Note: the related ``duply`` (and therefore
  ``duplicity``) command is always run with verbosity=error, regardless
  what has been set in ``duply_verbosity``. This is because at the usual
  verbosity=warning, on some machines, not understood exactly when, the
  command shows an apparently harmless message "No extraneous files
  found, nothing deleted in cleanup." The default for this variable is
  2; it can also be set to "" so that no incremental sets are removed.

duplicity_extra_parms
  Extra parameters to duplicity.  If you backup with ssh and have not
  added the target host key to known_hosts, you might want to use
  ``--ssh-options=-oStrictHostKeyChecking=no``.
