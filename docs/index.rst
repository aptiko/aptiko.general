aptiko.general Ansible collection
=================================

A collection of modules to setup apache, databases, backup and
email on Debian.

Installation
------------

::

    ansible-galaxy collection install git+https://github.com/aptiko/aptiko.general.git

Reference
---------

.. toctree::
   common
   duply
   :maxdepth: 1
   :caption: General roles:

.. toctree::
   apache
   apache_vhost
   nginx
   nginx_site
   :maxdepth: 1
   :caption: Web server roles:

.. toctree::
   dragonfly
   mail_satellite
   :maxdepth: 1
   :caption: Email roles:

.. toctree::
   mysql
   postgresql
   :maxdepth: 1
   :caption: Database roles:

.. toctree::
   node
   :maxdepth: 1
   :caption: Miscellaneous roles:

.. toctree::
   copyright
   :maxdepth: 1
   :caption: Miscellaneous:
