.. _nginx:

=====
nginx
=====

Overview
========

This is an Ansible role for installing nginx on Debian or Ubuntu. It
merely installs the nginx package and performs some essential
configuration such as allowing ports 80 and 443 through the firewall.
Other than that, it doesn't do much and should be combined with
:ref:`nginx_site`.

Options
=======

use_ferm
  If ``true`` (the default), it drops a configuration snippet in
  :file:`/etc/ferm/ansible-late` in order to allow connections to ports
  80 and 443.  In this case you must also use the :ref:`common` role.
