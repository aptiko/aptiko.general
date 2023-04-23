.. _dragonfly:

=========
dragonfly
=========

Description
===========

This is an Ansible role for installing the DragonFly Mail Agent, or dma,
on a Debianserver.  It also sets a catch-all alias in
:file:`/etc/aliases`.  It can be set to only send emails (useful for
enabling a machine to send administrative email, such as cron error
messages, to the admins).

**Deprecated:** dma can only accept email by running the ``sendmail``
command, which is what ``cron`` does. Most other programs need to use
SMTP via port 25, and dma does not listen to a port. Django can use dma,
but it needs a third-party library (``django-sendmail-backend``). To
avoid all that, use role :ref:`mail_satellite` instead.

Examples
========

Configuration example::

   admins: myemail@somewhere.com
   dma_smarthost: mail.itia.ntua.gr
   dma_smarthost_port: 587
   dma_smarthost_username: myuser
   dma_smarthost_password: topsecret
   dma_masquerade: sender@somewhere.com
   dma_defer: True
   dma_securetransfer: True
   dma_starttls: True

Parameters
==========

The following variables are used:

admins
  A comma-separated list of emails, to which all received
  emails without a domain will be sent.

dma_smarthost
  The smarthost.

dma_smarthost_port
  The port for the smarthost; usually 25, 465, or 587.

dma_smarthost_username, dma_smarthost_password
  Necessary when the smarthost requires authentication.

dma_masquerade
  Optional. The email address DMA will use for the envelope-from. Some
  smarthosts require the envelope-from to belong to the connected user.

dma_defer, dma_securetransfer, dma_starttls, dma_opportunistic_tls, dma_insecure
  By default these are False. If True, then they are set in the
  configuration file; e.g. if dma_starttls is true, STARTTLS is set in
  :file:`/etc/dma.conf`.
