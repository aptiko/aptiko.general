=====
mysql
=====

Overview
========

Installs MySQL on Debian/Ubuntu, and configures it so that user ``root``
can connect with a password (by default the operating system user
``root`` is allowed to connect as MySQL user ``root`` without a
password).

Package ``python3-pymysql`` is also installed to allow usage of the
Ansible ``community.mysql`` module.

Parameters
==========

mysql_root_password
  The password of the mysql ``root`` user.  Usually you will store this
  in the vault.

mysql_config
  Dictionary of configuration variables, like this::

    - role: aptiko.general.mysql
      mysql_config:
        disable-log-bin: null
        bind-address: 0.0.0.0

  The above will result in the following being added to
  :file:`/etc/mysql/mysql.conf.d/mysqld.cnf`::

      disable-log-bin
      bind-address = 0.0.0.0

  Those items that have the value ``null`` are merely added to the file
  without a trailing ``= [value]``.

mysql_allowed_client_ips
  The default is an empty string. If not an empty string, a ferm rule is
  added to allow mysql clients to connect to port 3306.  It should be a
  space-separated string of ip addresses.

prometheus_server_ips
  See the :ref:`prometheus` role.
