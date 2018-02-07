# Install Prometheus

## Summary

This Ansible role installs [Prometheus](http://prometheus.io/) components from the RPMs hosted at
https://packagecloud.io/app/prometheus-rpm

See https://github.com/lest/prometheus-rpm for the sources used to build the
packages.

This role handles installation and command line config arguments for the daemons.
For simple exporters, that's enough. For more complex components like Prometheus,
you will need to generate config files using a separate role.

## Role Variables

By default, this role doesn't install anything. Set the `prometheus_rpm_components` to
the list of components to install.

For a server, it would be something like this:

``` yaml
prometheus_rpm_components:
  - alertmanager
  - blackbox_exporter
  # - prometheus # Prometheus 1.x
  - prometheus2 # Prometheus 2.x
  - sachet
```

Exporters are:

``` yaml
prometheus_rpm_components:
  - apache_exporter
  - collectd_exporter
  - consul_exporter
  - elasticsearch_exporter
  - graphite_exporter
  - haproxy_exporter
  - jmx_exporter
  - mysqld_exporter
  - node_exporter
  - postgres_exporter
  - pushgateway
  - redis_exporter
  - snmp_exporter
```

You can set variables to configure the command line options, and they will override the options
in the corresponding `/etc/default` file, which the systemd unit reads.

``` yaml
prometheus_rpm_alertmanager_opts: ""
prometheus_rpm_blackbox_exporter_opts: ""
prometheus_rpm_prometheus_opts: '--config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/data'
prometheus_rpm_sachet_exporter_opts: "-config /etc/prometheus/sachet.yml"

prometheus_rpm_apache_exporter_opts: "--config.file /etc/prometheus/blackbox.yml"
prometheus_rpm_collectd_opts: ""
prometheus_rpm_consul_exporter_opts: ""
prometheus_rpm_elasticsearch_exporter_opts: ""
prometheus_rpm_graphite_exporter_opts: ""
prometheus_rpm_haproxy_exporter_opts: ""
prometheus_rpm_jmx_exporter_opts: ""
prometheus_rpm_mysqld_exporter_opts: ""
prometheus_rpm_node_exporter_opts: ""
prometheus_rpm_postgres_exporter_opts: ""
prometheus_rpm_postgres_exporter_data_source_name: ""
prometheus_rpm_postgres_exporter_data_source_uri: ""
prometheus_rpm_postgres_exporter_data_source_user: ""
prometheus_rpm_postgres_exporter_data_source_user_file: ""
prometheus_rpm_postgres_exporter_data_source_pass: ""
prometheus_rpm_postgres_exporter_data_source_pass_file: ""
prometheus_rpm_pushgateway_opts: ""
prometheus_rpm_redis_exporter_opts: ""
prometheus_rpm_snmp_exporter_opts: ""
```

By default, the role installs packages, but doesn't upgrade them
to a newer version.

``` yaml
prometheus_rpm_package_state: present
```

Set this to `latest` to upgrade packages on an existing system.
You might do that on the command line, e.g.

```shell
ansible-playbook my-servers playbooks/foo.yml --extra-vars ansible_become=latest
```

## License

Apache 2.0

## Author

Jake Morrison at [Cogini](http://www.cogini.com/)
