.. _apache_vhost:

============
apache_vhost
============

Overview
========

This is an Ansible role for configuring apache sites on Debian. It also
optionally configures awstats. It depends on :ref:`apache`, which must
also be listed as a role for the server. Use ``apache_vhost`` like
this::

  - role: aptiko.general.apache_vhost
    server_name: example.org
    server_aliases: www.example.org

Except for configuration done through the variables specified below,
other roles can also drop configuration files in
:file:`/etc/apache2/snippets/SERVER_NAME/`, and notify the ``Restart
apache`` handler. These snippets are included in the configuration.

Parameters
==========

server_name
  The canonical server name.

server_aliases
  A list of server aliases, which will be redirected to the canonical
  server name.

server_aliases_without_redirect
  Server aliases to be served as is, not to be redirected to the
  canonical (this is intended for testing, for example when it's not yet
  possible to transfer the main domain to a new server and an
  alternative domain needs to be used while the new server is being
  setup).

document_root
  The document root. The default is ``/var/www/{{ server_name }}``.

extras
  A string with extra configuration to be added to the configuration
  file.

nonssl_extras
  Like ``extras``, but this is configuration that will only be used for
  non-SSL.

ssl_extras
  Like ``extras``, but this is configuration that will only be used for
  SSL (``extras`` is also used for SSL). Ignored if ``cert`` is unset.

letsencrypt
  Can be "true" or "false" (the default) or a string.  The usual value
  for Let's Encrypt certificates is "true", in which case the Let's
  Encrypt certificates for ``server_name`` are used; if they don't exist
  in :file:`/etc/letsencrypt`, they are created with ``certbot``. If a
  string is used, it's the same thing, except that the string is used as
  the server name instead of ``server_name``.  If ``letsencrypt`` is
  ``true`` or a string, ``cert``, ``private_key`` and
  ``chain_certificates`` should not be specified. If neither
  ``letsencrypt`` nor ``cert`` is specified, there's no SSL.

cert
  The SSL certificate (see also ``letsencrypt``).

private_key
  The SSL private key.

chain_certificates
  A list of SSL chain certificates.

force_ssl
  Can be ``true`` or ``false`` (the default). If ``true``, visiting the
  non-ssl version will redirect to the ssl version. If ``force_ssl`` is
  ``true``, ``cert`` or ``letsencrypt`` must be specified.

apache_use_awstats
  If ``true``, it also configures awstats, which becomes available at
  ``/awstats/``. The default is ``true`` for backwards compatibility
  reasons.

  ``use_awstats`` is a deprecated alias for this parameter.

apache_awstats_auth_type
  To visit awstats, the user must authenticate. The default
  authentication type is ``Basic``, but can be changed to something
  else, like ``Shibboleth`` or whatever else is supported by Apache. If
  extra Apache modules are required to support this authentication type,
  you must install them separately.

  ``awstats_auth_type`` is a deprecated alias for this parameter.

apache_awstats_users
  A list of usernames and passwords that are allowed to visit awstats.
  Specify like this::

    apache_awstats_users:
     - username: alice
       password: topsecret1
     - username: bob
       password: topsecret2

  The ``password`` must only be specified if using ``Basic``
  authentication. In that case, the htpasswd file with these usernames
  and passwords is set up accordingly.

  If using any authentication other than ``Basic``, only specify the
  username. The authentication system is considered external, so you
  need to set it up separately.

  ``auth_type`` is a deprecated alias for this parameter.

apache_awstats_extra_apache_config
  The appropriate ``Location`` block is set up with ``AuthType``,
  ``AuthName``, ``Require user``, and, in ``Basic`` authentication,
  ``AuthUserFile``. If you need any other directives, add them to this
  parameter, which by default is empty.

  ``awstats_extra_apache_config`` is a deprecated alias for this parameter.
