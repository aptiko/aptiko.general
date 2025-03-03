.. _prometheus:

==========
prometheus
==========

Overview
========

This is an Ansible role for installing Prometheus and Grafana on
Debian/Ubuntu.

Parameters
==========

grafana_admin_password
  The password for the Grafana ``admin`` user; this is set when running
  the role.

prometheus_server_name
  The domain where Grafana will be listening, such as
  "prometheus.example.com".

prometheus_scrape_configs
  The ``scrape_configs`` part of the Prometheus configuration.

prometheus_server_ips
  A space-separated list of the IP addresses of the Prometheus server,
  typically an IPv4 and an IPv6 address. This is not actually used by
  this role, but by various other roles; when the parameter is set, then
  they install exporters as needed (e.g. the :ref:`common` role installs
  the node exporter; the :ref:`postgresql` role installs the postgresql
  exporter; and so on), and they allow these addresses through the
  firewall.

prometheus_alertmanager_config
  The alertmanager configuration (it will go to ``alertmanager.yml``). If
  unspecified, alertmanager will not be configured.

prometheus_alert_rules
  Configuration of the alerting rules (it will go to
  ``alert_rules.yml``). If unspecified, there will be no alert rules.

Example
=======

::

  tasks:
    - role: aptiko.general.prometheus
      grafana_admin_password: topsecret  # Should obviously be vaulted
      prometheus_server_name: prometheus.example.com
      prometheus_server_ips: "1.2.3.4 abcd:ef0:123:456::1"
      prometheus_scrape_configs:
        - job_name: prometheus
          scrape_interval: 5s
          scrape_timeout: 5s
          static_configs:
            - targets:
              - prometheus.example.com:9090

        - job_name: node
          static_configs:
            - targets:
              - host1.example.com:9100
              - host2.example.com:9100

        - job_name: postgresql
          static_configs:
            - targets:
              - host1.example.com:9187

        - job_name: mysql
          static_configs:
            - targets:
              - host2.example.com:9104
      prometheus_alertmanager_config:
        global:
          smtp_smarthost: 'localhost:25'
          smtp_from: 'noreply@example.com'
          smtp_require_tls: false
        templates:
        - '/etc/prometheus/alertmanager_templates/*.tmpl'
        route:
          group_by: ['alertname', 'cluster', 'service']
          group_wait: 60s
          group_interval: 10m
          repeat_interval: 3h
          receiver: email
          routes:
            - match_re:
                severity: '.*'
              receiver: email
        receivers:
        - name: 'email'
          email_configs:
          - to: 'sysadmin+prometheus@example.com'
            send_resolved: true
      prometheus_alert_rules:
        groups:
          - name: Machine down
            rules:
              - alert: Host down
                expr: up == 0
                for: 1m
                labels:
                  severity: critical
                annotations:
                  summary: Instance {{ "{{ " }} $labels.instance {{ "}}" }} is down"
