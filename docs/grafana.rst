.. _grafana:

=======
grafana
=======

Overview
========

This is an Ansible role for installing Grafana on Debian/Ubuntu.

Parameters
==========

.. data:: grafana_admin_password

   The password for the Grafana ``admin`` user; this is set when running
   the role.

.. data:: grafana_root_path

   The root path where Grafana will be served, such as "/grafana/". This
   must start and end with a slash. The default is "/".

.. data:: grafana_server_name

   The domain where Grafana will be listening, such as
   "grafana.example.com".

Example
=======

::

  tasks:
    - role: aptiko.general.grafana
      grafana_admin_password: topsecret  # Should be vaulted
      grafana_server_name: grafana.example.com
