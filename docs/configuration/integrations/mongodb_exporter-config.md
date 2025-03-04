+++
title = "mongodb_exporter_config"
+++

# mongodb_exporter_config

The `mongodb_exporter_config` block configures the `mongodb_exporter` integration, which is an embedded version of percona's [`mongodb_exporter`](https://github.com/percona/mongodb_exporter).

In order for this integration to work prorperly, you have to connect each node of your mongoDB cluster to an agent instance.
That's because this exporter does not collect metrics from multiple nodes.
Additionally, you need to define two custom label for you metrics using relabel_configs.
The first one is service_name, which is how you identify this node in your cluster (example: ReplicaSet1-Node1).
The second one is mongodb_cluster, which is the name of your mongodb cluster, and must be set the same value for all nodes composing the cluster (example: prod-cluster).
Here`s an example:

```yaml
relabel_configs:
    - source_labels: [__address__]
      target_label: service_name
      replacement: 'replicaset1-node1'
    - source_labels: [__address__]
      target_label: mongodb_cluster
      replacement: 'prod-cluster'
```

Besides that, there's not much to configure. Please refer to the full reference of options:

```yaml
  # Enables the mongodb_exporter integration
  [enabled: <boolean> | default = false]

  # Automatically collect metrics from this integration. If disabled,
  # the mongodb_exporter integration will be run but not scraped and thus not
  # remote-written. Metrics for the integration will be exposed at
  # /integrations/mongodb_exporter/metrics and can be scraped by an external
  # process.
  [scrape_integration: <boolean> | default = <integrations_config.scrape_integrations>]

  # How often should the metrics be collected? Defaults to
  # metrics.global.scrape_interval.
  [scrape_interval: <duration> | default = <global_config.scrape_interval>]

  # The timeout before considering the scrape a failure. Defaults to
  # metrics.global.scrape_timeout.
  [scrape_timeout: <duration> | default = <global_config.scrape_timeout>]

  # Allows for relabeling labels on the target.
  relabel_configs:
    [- <relabel_config> ... ]

  # Relabel metrics coming from the integration, allowing to drop series
  # from the integration that you don't care about.
  metric_relabel_configs:
    [ - <relabel_config> ... ]

  # How frequent to truncate the WAL for this integration.
  [wal_truncate_frequency: <duration> | default = "60m"]

  # Monitor the exporter itself and include those metrics in the results.
  [include_exporter_metrics: <bool> | default = false]

  #
  # Exporter-specific configuration options
  #

  # MongoDB node connection URL, which must be in the [`Standard Connection String Format`](https://docs.mongodb.com/manual/reference/connection-string/#std-label-connections-standard-connection-string-format)
  [mongodb_uri: <string>]
```
