.. _node:

====
node
====

Overview
========

This is an Ansible role for installing node.js on Debian/Ubuntu. It uses
apt for this, but installs a specific version of node from a nodesource
repository, as node.js versions for the default Debian/Ubuntu
repositories are typically too old.

Parameters
==========

node_version
  The default is 20. You can use another number such as 18 or 21.
