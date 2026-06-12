.. _webserver:

=========
webserver
=========

Overview
========

This role installs Apache or nginx. It also performs some essential
configuration such as enabling several apache modules, marking 301
redirections as non-cacheable, and changing the default log format. It
can also set up a Let's Encrypt wildcard certificate and a "default"
site suitable for serving such wildcard sites. Unless such a wildcard
setup is needed, the role doesn't do much and should be combined with
:ref:`website`.

Parameters
==========

webserver_type
  Must be either ``apache`` or ``nginx``. This parameter is required.

webserver_use_ferm
  If ``true`` (the default), it drops a configuration snippet in
  :file:`/etc/ferm/ansible-late` in order to allow connections to ports
  80 and 443. In this case you must also use the :ref:`common` role.

webserver_use_awstats
  If ``true``, it installs awstats and performs essential global
  configuration. The default is ``false``. Currently only implemented for
  Apache.

webserver_awstats_domains
  A list of domain names for awstats on the default site. If the
  list is empty, no awstats site is configured for the default
  site. Currently only implemented for Apache.

webserver_default_site_awstats_auth_type
  Authentication type for the default-site awstats. The default is
  ``Basic``. Currently only implemented for Apache.

webserver_default_site_awstats_users
  Users allowed to visit awstats for the default site. Currently only
  implemented for Apache.

webserver_default_site_awstats_extra_config
  Extra directives for the default-site awstats location block.
  Currently only implemented for Apache.

webserver_default_site_document_root
  Document root for the default site. The default is ``/var/www/html``.

webserver_default_site_extras
  Extra configuration appended to the default-site configuration.
  The default is to return 404 for all visits (e.g. for Apache it is
  ``Redirect 404 /``).

webserver_default_site_letsencrypt_domain
  Wildcard DNS name for the default-site certificate. If set,
  certbot creates the certificate if needed and the default site uses
  it. Currently only implemented for Apache.

webserver_cloudflare_api_token
  Cloudflare API token used for the default-site DNS challenge.
  Currently only implemented for Apache.

webserver_default_site_force_ssl
  If ``true``, the default site redirects HTTP to HTTPS.

webserver_letsencrypt_admin
  Email address used by certbot for the default-site certificate.
