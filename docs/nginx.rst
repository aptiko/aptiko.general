.. _nginx:

=====
nginx
=====

Overview
========

This role is deprecated. Use :ref:`webserver` with
``webserver_type: nginx`` instead.

This is an Ansible role for installing nginx on Debian or Ubuntu. When
:data:`base_setup_firewall` is enabled, it allows incoming HTTP and HTTPS
connections through the firewall. Other than that, it doesn't do much
and should be combined with :ref:`nginx_site`.
