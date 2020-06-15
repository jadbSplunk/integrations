
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/templates/monitor-page.md.tmpl --->

# kubelet-metrics

Monitor Type: `kubelet-metrics` ([Source](https://github.com/signalfx/signalfx-agent/tree/master/pkg/monitors/kubernetes/kubeletmetrics))

**Accepts Endpoints**: **Yes**

**Multiple Instances Allowed**: Yes

## Overview

This monitor pulls container metrics from a Kubernetes kubelet instance via
the `/stats/summary` endpoint.

This montior is intended to replace the `kubelet-stats` monitor that
relied on now deprecated endpoints in the kubelet.  It does not get as
many metrics but the important ones are there.

Metrics about container cpu/memory limits and requests come from the
[kubernetes-cluster](./kubernetes-cluster.md) monitor.

For the mounted volumes metrics that are also emitted by the
`/stats/summary` endpoint, see the
[kubernetes-volumes](./kubernetes-volumes.md) monitor.

To monitor the kubelet instance itself, via the `/metrics` endpoint, use
the [prometheus-exporter](./prometheus-exporter.md) monitor and point it to
that endpoint.


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: kubelet-metrics
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `kubeletAPI` | no | `object (see below)` | Kubelet client configuration |
| `usePodsEndpoint` | no | `bool` | If true, this montior will scrape additional metadata from the `/pods` endpoint on the kubelet to enhance the container metrics with the `container_id` dimension that is not otherwise available from the `/stats/summary` endpoint. (**default:** `false`) |


The **nested** `kubeletAPI` config object has the following fields:

| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `url` | no | `string` | URL of the Kubelet instance.  This will default to `http://<current node hostname>:10255` if not provided. |
| `authType` | no | `string` | Can be `none` for no auth, `tls` for TLS client cert auth, or `serviceAccount` to use the pod's default service account token to authenticate. (**default:** `none`) |
| `skipVerify` | no | `bool` | Whether to skip verification of the Kubelet's TLS cert (**default:** `true`) |
| `caCertPath` | no | `string` | Path to the CA cert that has signed the Kubelet's TLS cert, unnecessary if `skipVerify` is set to false. |
| `clientCertPath` | no | `string` | Path to the client TLS cert to use if `authType` is set to `tls` |
| `clientKeyPath` | no | `string` | Path to the client TLS key to use if `authType` is set to `tls` |
| `logResponses` | no | `bool` | Whether to log the raw cadvisor response at the debug level for debugging purposes. (**default:** `false`) |


## Metrics

These are the metrics available for this monitor.
Metrics that are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
(*default*) are ***in bold and italics*** in the list below.


 - ***`container_cpu_utilization`*** (*cumulative*)<br>    Cumulative cpu utilization in percentages.  This is equivalent to "centicores", or hundreths of CPU cores consumed.  This metric is **NOT** normalized by the total # of cores on the system.
 - ***`container_fs_available_bytes`*** (*gauge*)<br>    The storage space available (bytes) for the container root filesystem
 - ***`container_fs_capacity_bytes`*** (*gauge*)<br>    The total capacity (bytes) of the container root filesystem's underlying storage
 - ***`container_fs_usage_bytes`*** (*gauge*)<br>    This is the bytes used by the container rootfs on the filesystem. This
    may differ from the total bytes used on the filesystem and may not
    equal container_fs_capacity_bytes - container_fs_available_bytes.

 - `container_memory_available_bytes` (*gauge*)<br>    Available memory for use.  This is defined as the memory limit -
    container_memory_working_set_bytes. If memory limit is undefined, this
    metric is omitted.

 - `container_memory_major_page_faults` (*cumulative*)<br>    Cumulative number of major page faults.
 - `container_memory_page_faults` (*cumulative*)<br>    Cumulative number of minor page faults.
 - `container_memory_rss_bytes` (*gauge*)<br>    The amount of anonymous and swap cache memory (includes transparent hugepages)
 - ***`container_memory_usage_bytes`*** (*gauge*)<br>    Total memory in use -- this includes all memory regardless of when it was accessed.
 - `container_memory_working_set_bytes` (*gauge*)<br>    The amount of working set memory. This includes recently accessed
    memory, dirty memory, and kernel memory. container_memory_working_set_bytes is <=
    container_memory_usage_bytes.

 - ***`pod_network_receive_bytes_total`*** (*cumulative*)<br>    Cumulative count of bytes received. **Note that this metric is not emitted when using the cri-o container runtime.**
 - ***`pod_network_receive_errors_total`*** (*cumulative*)<br>    Cumulative count of errors encountered while receiving. **Note that this metric is not emitted when using the cri-o container runtime.**
 - ***`pod_network_transmit_bytes_total`*** (*cumulative*)<br>    Cumulative count of bytes transmitted. **Note that this metric is not emitted when using the cri-o container runtime.**
 - ***`pod_network_transmit_errors_total`*** (*cumulative*)<br>    Cumulative count of errors encountered while transmitting. **Note that this metric is not emitted when using the cri-o container runtime.**

#### Group podEphemeralStats
All of the following metrics are part of the `podEphemeralStats` metric group. All of
the non-default metrics below can be turned on by adding `podEphemeralStats` to the
monitor config option `extraGroups`:
 - `pod_ephemeral_storage_capacity_bytes` (*gauge*)<br>    Represents the storage space available (bytes) for the filesystem. This value is a combination of total filesystem usage for the containers and emptyDir-backed volumes in the measured Pod. See more about emptyDir-backed volumes https://kubernetes.io/docs/concepts/storage/volumes/#emptydir
 - `pod_ephemeral_storage_used_bytes` (*gauge*)<br>    Represents the bytes used on the filesystem. This value is a total filesystem usage for the containers and emptyDir-backed volumes in the measured Pod. See more about emptyDir-backed volumes https://kubernetes.io/docs/concepts/storage/volumes/#emptydir

### Non-default metrics (version 4.7.0+)

To emit metrics that are not _default_, you can add those metrics in the
generic monitor-level `extraMetrics` config option.  Metrics that are derived
from specific configuration options that do not appear in the above list of
metrics do not need to be added to `extraMetrics`.

To see a list of metrics that will be emitted you can run `agent-status
monitors` after configuring this monitor in a running agent instance.


