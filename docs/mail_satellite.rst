.. _mail_satellite:

==============
mail_satellite
==============

Overview
========

Installs and configures a mail server so that it's possible to send
emails either through port 25 or by executing the ``sendmail`` command.
All email is sent using a
smarthost.

It's mainly useful in two cases:

1. Forwarding cron email (otherwise the administrator would need to
   login to see if any cron email has been received locally.)
2. Being used as the smarthost of applications or services installed on
   the server. In this case such applications should be configured to
   use ``localhost:25`` as the smarthost. While they could be configured
   to use the smarthost directly, using ``localhost:25`` can be useful
   especially when the smarthost is not in our control, because whenever
   something doesn't work we can get some information from Postfix's
   logs as to what is going wrong.

Parameters
==========

mailname
  The default domain name that will be used for email addresses that do
  not contain ``@``.

smarthost
  The smart host, such as ``relay.example.com``.

smarthostport
  The smarthost port, such as 25 or 587 (the default). If the outgoing
  port is 465 or 587, encrypted connections are forced.

smarthostusername, smarthostpassword
  The username and password to connect to the smart host. If unspecified
  it will be connecting unauthenticated.

masquerade_domains
  Optional. This will be used as Postfix's masquerade_domains_
  parameter.  For example, if ``mailname`` is
  ``nextcloud.digigov.grnet.gr``, you may want to use
  ``masquerade_domains = grnet.gr`` so that the sender
  ``root@nextcloud.digigov.grnet.gr`` will be converted to ``grnet.gr``.

  .. _masquerade_domains: http://www.postfix.org/postconf.5.html#masquerade_domains

mail_aliases
  A hash that maps local emails to actual addresses, for example::

    mail_aliases:
      root: antonis@example.com,panagiotis@example.com
      www-data: root

  You should practically always create an alias for ``root``, and very
  often for ``www-data`` as well, and for everything that uses cron.

  Note: We don't use :file:`/etc/aliases` for this functionality;
  Postfix only uses :file:`/etc/aliases` for local delivery. We use
  virtual_alias_maps_ instead.

  .. _virtual_alias_maps: http://www.postfix.org/postconf.5.html#virtual_alias_maps

inet_interfaces
  If ``loopback-only`` (the default), it listens only on the local
  interface. Change it to ``all`` (or any value accepted by the postfix
  ``inet_interfaces`` parameter) so that it listens on all interfaces.
  In that case, you need to care about the firewall yourself. For
  example, assuming you use the :ref:`common` firewall::

    - name: Allow smtp through firewall
      lineinfile:
        path: /etc/ferm/ansible-late
        line: "proto tcp dport smtp saddr (1.2.3.4 5.6.7.8) ACCEPT;"
      notify: Reload ferm

  You also need to set ``my_networks``.

mynetworks
  The networks that are allowed to relay. If unset, only the localhost
  from the local interface is allowed to send emails. If you set it,
  don't specify the localhost, this will be included anyway.

  See also ``inet_interfaces``.
