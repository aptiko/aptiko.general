.. _postgresql:

==========
postgresql
==========

Overview
========

This is an Ansible role for installing PostgreSQL on Debian. It makes
sure the installation is done under the locale en_us.UTF-8. It also
installs PostGIS, creates a ``template_postgis`` database template, and
sets the ``postgres`` password. It assumes the ``duply`` role is also
installed, and creates a pre-backup script that dumps all databases into
``/var/backups``.

Parameters
==========

use_timescale
  Default ``false``. Set to ``true`` to install Timescale.

postgres_password
  The password of the ``postgres`` database user.  Store this in the
  vault. The role sets the ``postgres`` database user to have this
  password.

prometheus_server_ips
  See the :ref:`prometheus` role.
