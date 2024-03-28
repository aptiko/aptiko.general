.. _common:

======
common
======

Overview
========

This is an Ansible role for applying common configuration to all Debian
machines. It installs sudo, ntp, pip and ca-certificates, generates
locales en_US.UTF-8 and en_DK.UTF-8, configures firewall to deny all but
allow port 22 (additional rules can be specified by other roles; see
below), installs the root ssh keys and authorized keys, configures ssh
to allow root to login without password, and sets some options for
root's shell in :file:`.profile` and :file:`.bashrc`.

Parameters
==========

ssh_pub_key, ssh_priv_key
  Optional. Root's ssh keys.

root_authorized_keys
  Optional. A list of strings. Any other keys are removed from root's
  authorized_keys. If unspecified, the root's authorized keys are not
  touched.

ssh_port
  Optional. The port on which the ssh server will be listening. The
  default is 22. If this is changed, you will then need to reconfigure
  Ansible so that, in next runs, it connects to the new port.

ssh_allowed_ip_addresses
  Optional. A list of IP addresses or networks from which access to ssh
  will be allowed. The list can includes both IPv4 and IPv6 addresses
  and networks. The default is to allow access to all addresses. This
  affects the configuration of the firewall.

command_line_editing_mode
  Optional. Set it to "vi" to enable vi editing mode in bash.

prometheus_server_ips
  See the :ref:`prometheus` role.

forward_journal_to_syslog
  By default, Debian systems forward journal entries to syslog, so they
  are duplicated in :file:`/var/log/syslog`. Setting this to ``false``
  deactivates this. The default is ``true``.

Firewall
========

The role installs ferm. If, in another role or play, you need to add a
firewall rule, add a line to :file:`/etc/ferm/ansible-late`, like this::

    - name: Allow http and https through firewall
      lineinfile:
        path: /etc/ferm/ansible-late
        line: "proto tcp dport (http https) ACCEPT;"
      notify: Reload ferm

The file :file:`/etc/ferm/ansible-late` is appropriate for such
additional ACCEPT rules. If you want a rule to be applied early, use
:file:`/etc/ferm/ansible-early` instead. This is useful for DROP rules.
Example::

    - name: Cut misbehaving machine at the firewall
      lineinfile:
        path: /etc/ferm/ansible-early
        line: "saddr (18.19.20.21 2a01:4f8:2a01:4f8::/32) DROP;"
      notify: Reload ferm

OBVIOUS WARNING: Make an error and you're locked out!
