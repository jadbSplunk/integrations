
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/monitor-page.md.tmpl --->

# statsd

Monitor Type: `statsd` ([Source](https://github.com/signalfx/signalfx-agent/tree/master/internal/monitors/statsd))

**Accepts Endpoints**: No

**Multiple Instances Allowed**: Yes

## Overview

This monitor will receive and aggergate Statsd metrics and convert them to
datapoints.  It listens on a configured address and port in order to
receive the statsd metrics.  Note that this monitor does not support statsd
extensions such as tags.

Supports Spark 2.2.0 and later.

The monitor supports the `Counter`, `Timer`, `Gauge` and `Set` types which
are dispatched as the SignalFx types `counter`, `gauge`, `gauge` and
`gauge` respectively.

**Note that datapoints will get a `host` dimension of the current host that
the agent is running on, not the host from which the statsd metric was
sent.  For this reason, it is recommended to send statsd metrics to a local
agent instance.**

<!--- SETUP --->
#### Verifying installation

You can send StatsD metrics locally with `netcat` as follows, then verify
in SignalFx that the metric arrived (assuming the default config).

```
$ echo "statsd.test:1|g" | nc -w 1 -u 127.0.0.1 8125
```

<!--- SETUP --->
#### Adding dimensions to StatsD metrics

The StatsD monitor can parse keywords from a statsd metric name by a set of
converters that was configured by user.

```
converters:
  - pattern: "cluster.cds_{traffic}_{mesh}_{service}-vn_{}.{action}"
    ...
```

This converter will parse `traffic`, `mesh`, `service` and `action` as dimensions
from a metric name `cluster.cds_egress_ecommerce-demo-mesh_gateway-vn_tcp_8080.update_success`.
If a section has only a pair of brackets without a name, it will not capture a dimension.

When multiple converters were provided, a metric will be converted by the first converter with a
matching pattern to the metric name.

<!--- SETUP --->
#### Formatting metric name

You can customize a metric name by providing a format string within the converter configuration.

```
converters:
  - pattern: "cluster.cds_{traffic}_{mesh}_{service}-vn_{}.{action}"
    metric: "{traffic}.{action}"
```

The metrics which match to the given pattern will be reported to SignalFx as `{traffic}.{action}`.
For instance, metric `cluster.cds_egress_ecommerce-demo-mesh_gateway-vn_tcp_8080.update_success`
will be reported as `egress.update_success`.

`metric` is required for a converter configuration. A converter will be disabled if `metric` is not provided.

```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: statsd
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `listenAddress` | no | `string` | The host/address on which to bind the UDP listener that accepts statsd datagrams (**default:** `localhost`) |
| `listenPort` | no | `integer` | The port on which to listen for statsd messages (**default:** `8125`) |
| `metricPrefix` | no | `string` | A prefix in metric names that needs to be removed before metric name conversion |
| `converters` | no | `list of objects (see below)` | A list converters to convert StatsD metric names into SignalFx metric names and dimensions |


The **nested** `converters` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `pattern` | no | `string` | A pattern to match against StatsD metric names |
| `metricName` | no | `string` | A format to compose a metric name to report to SignalFx |



The agent does not do any built-in filtering of metrics coming out of this
monitor.

