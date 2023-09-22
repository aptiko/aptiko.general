.. _nginx_site:

==========
nginx-site
==========

Overview
========

This is an Ansible role for configuring nginx sites (actually domains,
which on nginx also have the unfortunate name "servers") on Debian or
Ubuntu.  It depends on :ref:`nginx`, which must also be listed as a role
for the server. The minimal way to use it is this::

  - role: nginx-site
    server_name: example.org

but the most usual will be like this::

  - role: nginx_site
    server_name: example.org
    server_aliases:
      - www.example.org
    letsencrypt: true
    force_ssl: true

Except for configuration done through the variables specified below,
other roles can also drop configuration files in
:file:`/etc/nginx/snippets/DOMAIN_NAME/`, and notify the ``Reload
nginx`` handler. These snippets are included in the configuration.

Parameters
==========

server_name
  The canonical domain name.

server_aliases
  A list of domain aliases, which will be redirected to the canonical
  server name.

server_aliases_without_redirect
  Domain aliases to be served as is, not to be redirected to the
  canonical (this is intended for testing, for example when it's not yet
  possible to transfer the main domain to a new server and an
  alternative domain needs to be used while the new server is being
  setup).

document_root
  The document root. The default is ``/var/www/{{ server_name }}``.

extras
  A string with extra configuration to be added to the configuration
  file. You should generally not use this; dropping a snippet in
  `/etc/nginx/snippets/SITE_NAME/` is usually a better solution. 

nonssl_extras
  Like ``extras``, but this is configuration that will only be used for
  non-SSL.  You are unlikely to need this; normally ``extras`` should
  suffice.

ssl_extras
  Like ``extras``, but this is configuration that will only be used for
  SSL (``extras`` is also used for SSL). Ignored if ``cert`` is unset.
  You are unlikely to need this; normally ``extras`` should suffice.

letsencrypt
  Can be "true" or "false" (the default) or a string.  The usual value
  for Let's Encrypt certificates is "true", in which case the Let's
  Encrypt certificates for ``server_name`` are used; if they don't exist
  in :file:`/etc/letsencrypt`, they are created with ``certbot``. If a
  string is used, it's the same thing, except that the string is used as
  the domain name instead of ``server_name``.  If `letsencrypt` is
  "false", there's no SSL.

letsencrypt_admin
  The email that is registered with certbot. This must be specified if
  ``letsencrypt`` is specified. Let's Encrypt will email that address if
  the certificate fails to renew.

force_ssl
  Can be "true" or "false" (the default). If "true", visiting the
  non-ssl version will redirect to the ssl version. If ``force_ssl`` is
  "true", ``letsencrypt`` must be specified.
