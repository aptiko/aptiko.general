.. _docker:

======
docker
======

Overview
========

This is an Ansible role for installing Docker on Debian/Ubuntu. It merely
installs it. To configure it, use the ``community.docker`` collection.

Example
=======

::

  tasks:
    - aptiko.general.docker
