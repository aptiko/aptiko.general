.. _common:

======
common
======

This role is deprecated. Use :ref:`base` instead.

Overview
========

This is an Ansible role for applying common configuration to all Debian
machines. It installs sudo, ntp, pip and ca-certificates, generates
locales en_US.UTF-8 and en_DK.UTF-8, installs the root ssh keys and
authorized keys, configures ssh to allow root to login without password,
and sets some options for root's shell in :file:`.profile` and
:file:`.bashrc`. It can also configure nftables.

Parameters
==========

.. data:: ssh_pub_key
          ssh_priv_key

   Optional. Root's RSA ssh keys.

.. data:: root_authorized_keys

   Optional. A list of strings. Any other keys are removed from root's
   authorized_keys. If unspecified, the root's authorized keys are not
   touched.

.. data:: ssh_port

   Optional. The port on which the ssh server will be listening. The
   default is 22. If this is changed, you will then need to reconfigure
   Ansible so that, in next runs, it connects to the new port.

.. data:: ssh_allowed_ip_addresses

   Optional. A list of IP addresses or networks from which access to ssh
   will be allowed. The list can includes both IPv4 and IPv6 addresses
   and networks. The default is to allow access to all addresses. This
   affects the configuration of the firewall.

.. data:: base_setup_firewall

   Optional. Whether to configure nftables. The default is ``false``.

.. data:: command_line_editing_mode

   Optional. Set it to "vi" to enable vi editing mode in bash.

.. data:: prometheus_server_ips

   See the :ref:`prometheus` role.

.. data:: forward_journal_to_syslog

   By default, Debian systems forward journal entries to syslog, so they
   are duplicated in :file:`/var/log/syslog`. Setting this to ``false``
   deactivates this. The default is ``true``.

Updates
=======

The role contains an update task set that performs ``apt update``, ``apt
upgrade``, ``apt autoremove``, and checks whether a restart is required.
Run it like this (this will restart the machine if needed)::

    ansible-playbook playbook.yml --tags update,restart

If you don't specify the ``restart`` tag, the machines will not be
automatically restarted; only a warning will be shown if a restart is
needed.

Firewall
========

When :data:`base_setup_firewall` is ``true``, the role installs nftables
and removes ferm. If, in another role or play, you need to add a firewall
rule, add a line to
:file:`/etc/nftables/ansible-late.nft`, like this::

    - name: Allow http and https through firewall
      lineinfile:
        path: /etc/nftables/ansible-late.nft
        line: "tcp dport { http, https } accept"
      notify: Reload nftables

The file :file:`/etc/nftables/ansible-late.nft` is appropriate for such
additional ACCEPT rules. If you want a rule to be applied early, use
:file:`/etc/nftables/ansible-early.nft` instead. This is useful for DROP
rules. Example::

    - name: Cut misbehaving machine at the firewall
      lineinfile:
        path: /etc/nftables/ansible-early.nft
        line: "ip saddr 18.19.20.21 drop"
      notify: Reload nftables

OBVIOUS WARNING: Make an error and you're locked out!
