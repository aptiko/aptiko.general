.. _website:

=======
website
=======

Overview
========

This is an Ansible role for configuring websites on Debian with either
Apache or Nginx. It also optionally configures awstats for Apache. Use
it like this::

  - role: aptiko.general.webserver
    webserver_type: nginx

  - role: aptiko.general.website
    webserver_type: nginx
    website_domain: example.org
    website_ssl: letsencrypt
    website_force_ssl: true

Other roles can drop snippets in :file:`/etc/apache2/snippets/SERVER_NAME/`
or :file:`/etc/nginx/snippets/SERVER_NAME/` and notify the matching
handler.

Parameters
==========

webserver_type
  Required. Must be either ``apache`` or ``nginx``. This is actually a parameter
  of the :ref:`webserver` role.

website_domain
  Canonical site name.

website_domain_aliases
  Aliases redirected to the canonical site.

website_domain_aliases_without_redirect
  Aliases served as-is instead of being redirected.

website_document_root
  Document root. The default is ``/var/www/{{ website_domain }}``.

website_extras
  Extra configuration added to the generated site configuration.

website_nonssl_extras
  Extra configuration used only for non-SSL HTTP.

website_ssl_extras
  Extra configuration used only for SSL.

website_ssl
  Can be ``off``, ``letsencrypt``, ``self-signed`` or ``custom``.
  The default is ``self-signed``.

  If ``off``, SSL is disabled.

  If ``letsencrypt``, certbot is used and the site uses the resulting
  certificate.

  If ``self-signed``, Debian's snakeoil certificate is used.

  If ``custom``, ``website_cert``, ``website_private_key`` and
  ``website_chain_certificates`` are used.

website_letsencrypt_domain
  The domain used when requesting a Let's Encrypt certificate. The
  default is ``website_domain``.

website_letsencrypt_admin
  Email address registered with certbot.

website_force_ssl
  If ``true``, HTTP redirects to HTTPS. The default is ``true``
  (provided ssl is used).

website_cert
  SSL certificate content for custom certificates.

website_private_key
  SSL private key for custom certificates.

website_chain_certificates
  List of chain certificates for custom certificates.

website_use_awstats
  Enables awstats configuration for the website. The default is
  ``false``. Currently implemented only for Apache.

website_awstats_auth_type
  Authentication type for awstats. The default is ``Basic``. Currently
  implemented only for Apache.

website_awstats_users
  Users allowed to visit awstats. Currently implemented only for Apache.

website_awstats_extra_config
  Extra directives for the website awstats location block. Currently
  implemented only for Apache.
