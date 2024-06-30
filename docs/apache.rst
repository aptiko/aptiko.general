.. _apache:

======
apache
======

Overview
========

This is an Ansible role for installing apache on Debian. It also
optionally installs awstats.

It performs some essential configuration such as enabling several
apache modules, marking 301 redirections as non-cacheable, and changing
the default log format. It can also set up a Let's Encrypt wildcard
certificate and a "default" site suitable for serving such wildcard
sites. Unless such a wildcard setup is needed, the role doesn't do much
and should be combined with :ref:`apache_vhost`.

Parameters
==========

apache_use_ferm
  If ``true`` (the default), it drops a configuration snippet in
  :file:`/etc/ferm/ansible-late` in order to allow connections to ports
  80 and 443.  In this case you must also use the :ref:`common` role.

  ``use_ferm`` is a deprecated alias for this parameter.

apache_use_awstats
  If ``true``, it installs awstats and performs some essential global
  (i.e. not site-specific) configuration. The default is ``true`` for
  backwards compatibility reasons.

  ``use_awstats`` is a deprecated alias for this parameter.

apache_awstats_domains
  This is a list of domain names. Unfortunately awstats does not support
  wildcard domains at the time of this writing. Thus, each different
  domain needs to be added to this list. If the list is empty, no
  awstats site is configured for the apache default site. (It may still
  be configured for apache sites enabled using the ``apache_vhost``
  role. 

apache_default_site_awstats_auth_type
  To visit awstats, the user must authenticate. The default
  authentication type is ``Basic``, but can be changed to something
  else, like ``Shibboleth`` or whatever else is supported by Apache. If
  extra Apache modules are required to support this authentication type,
  you must install them separately.

apache_default_site_awstats_users
  A list of usernames and passwords that are allowed to visit awstats
  for the default site.  Specify like this::

    apache_default_site_awstats_users:
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

apache_default_site_awstats_extra_apache_config
  The appropriate ``Location`` block is set up with ``AuthType``,
  ``AuthName``, ``Require user``, and, in ``Basic`` authentication,
  ``AuthUserFile``. If you need any other directives, add them to this
  parameter, which by default is empty.

apache_default_site_document_root
  The document root for the default site. The default is
  ``/var/www/html``.

apache_default_site_extras
  A string with extra configuration to be added to the configuration
  file for the default site. The default is ``Redirect 404 /``.

apache_default_site_letsencrypt_server_name
  If this is specified, it should be a wildcard DNS like
  ``*.example.com``. The certificate will be created with certbot if it
  does not already exist. The default site is configured to use it. The
  default value for this parameter is ``false``, meaning no SSL will be
  supported for the default site.

  Currently only DNS verification with Cloudflare is supported; see
  ``apache_cloudflare_api_token``.

apache_cloudflare_api_token
  If ``apache_default_site_letsencrypt_server_name`` is specified,
  ``certbot`` is configured to perform DNS verification with Cloudflare.
  This is the credentials for Cloudflare. It should be vaulted.

apache_default_site_force_ssl
  Can be ``true`` or ``false`` (the default). If ``true``, visiting the
  non-ssl version of the default site will redirect to the ssl version.
  If it is ``true``, ``apache_default_site_letsencrypt_server_name``
  should be specified.
