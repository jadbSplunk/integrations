
<!--- Generated by to-integrations-repo script in Smart Agent repo, DO NOT MODIFY HERE --->
<!--- GENERATED BY gomplate from scripts/docs/monitor-page.md.tmpl --->

# telegraf/procstat

Monitor Type: `telegraf/procstat` ([Source](https://github.com/signalfx/signalfx-agent/tree/master/internal/monitors/telegraf/monitors/procstat))

**Accepts Endpoints**: No

**Multiple Instances Allowed**: Yes

## Overview

This monitor reports metrics about processes.
This monitor is based on the Telegraf procstat plugin.  More information about the Telegraf plugin
can be found [here](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/procstat).

Please note that the Smart Agent only supports the `native` pid finder and the options
`cgroup` and `systemd unit` are not supported at this time.

On Linux hosts, this monitor relies on the `/proc` filesystem.
If the underlying host's `/proc` file system is mounted somewhere other than
/proc please specify the path using the top level configuration `procPath`.

Sample Yaml Configuration

```yaml
procPath: /proc
monitors:
 - type: telegraf/procstat
   exe: "signalfx-agent*"
```


## Configuration

To activate this monitor in the Smart Agent, add the following to your
agent config:

```
monitors:  # All monitor config goes under this key
 - type: telegraf/procstat
   ...  # Additional config
```

**For a list of monitor options that are common to all monitors, see [Common
Configuration](../monitor-config.html#common-configuration).**


| Config option | Required | Type | Description |
| --- | --- | --- | --- |
| `exe` | no | `string` | The name of an executable to monitor.  (ie: `exe: "signalfx-agent*"`) |
| `pattern` | no | `string` | Pattern to match against.  On Windows the pattern should be in the form of a WMI query. (ie: `pattern: "%signalfx-agent%"`) |
| `user` | no | `string` | Username to match against |
| `pidFile` | no | `string` | Path to Pid file to monitor.  (ie: `pidFile: "/var/run/signalfx-agent.pid"`) |
| `processName` | no | `string` | Used to override the process name dimension |
| `prefix` | no | `string` | Prefix to be added to each dimension |
| `pidTag` | no | `bool` | Whether to add PID as a dimension instead of part of the metric name (**default:** `false`) |
| `cGroup` | no | `string` | The name of the cgroup to monitor.  This cgroup name will be appended to the configured `sysPath`.  See the agent config schema for more information about the `sysPath` agent configuration. |
| `WinService` | no | `string` | The name of a windows service to report procstat information on. |


## Metrics

These are the metrics available for this monitor.
This monitor emits all metrics by default; however, **none are categorized as
[container/host](https://docs.signalfx.com/en/latest/admin-guide/usage.html#about-custom-bundled-and-high-resolution-metrics)
-- they are all custom**.



 - ***`procstat.cpu_time`*** (*gauge*)<br>    Amount of cpu time consumed by the process.
 - ***`procstat.cpu_usage`*** (*gauge*)<br>    CPU used by the process.
 - ***`procstat.involuntary_context_switches`*** (*gauge*)<br>    Number of involuntary context switches.
 - ***`procstat.memory_data`*** (*gauge*)<br>    VMData memory used by the process.
 - ***`procstat.memory_locked`*** (*gauge*)<br>    VMLocked memory used by the process.
 - ***`procstat.memory_rss`*** (*gauge*)<br>    VMRSS memory used by the process.
 - ***`procstat.memory_stack`*** (*gauge*)<br>    VMStack memory used by the process.
 - ***`procstat.memory_swap`*** (*gauge*)<br>    VMSwap memory used by the process.
 - ***`procstat.memory_vms`*** (*gauge*)<br>    VMS memory used by the process.
 - ***`procstat.nice_priority`*** (*gauge*)<br>    Nice priority number of the process.
 - ***`procstat.num_fds`*** (*gauge*)<br>    Number of file descriptors.  This may require the agent to be running as root.
 - ***`procstat.num_threads`*** (*gauge*)<br>    Number of threads used by the process.
 - ***`procstat.read_bytes`*** (*gauge*)<br>    Number of bytes read by the process.  This may require the agent to be running as root.
 - ***`procstat.read_count`*** (*gauge*)<br>    Number of read operations by the process.  This may require the agent to be running as root.
 - ***`procstat.realtime_priority`*** (*gauge*)<br>    Real time priority of the process.
 - ***`procstat.rlimit_cpu_time_hard`*** (*gauge*)<br>    The hard cpu rlimit.
 - ***`procstat.rlimit_cpu_time_soft`*** (*gauge*)<br>    The soft cpu rlimit.
 - ***`procstat.rlimit_file_locks_hard`*** (*gauge*)<br>    The hard file lock rlimit.
 - ***`procstat.rlimit_file_locks_soft`*** (*gauge*)<br>    The soft file lock rlimit.
 - ***`procstat.rlimit_memory_data_hard`*** (*gauge*)<br>    The hard data memory rlimit.
 - ***`procstat.rlimit_memory_data_soft`*** (*gauge*)<br>    The soft data memory rlimit.
 - ***`procstat.rlimit_memory_locked_hard`*** (*gauge*)<br>    The hard locked memory rlimit.
 - ***`procstat.rlimit_memory_locked_soft`*** (*gauge*)<br>    The soft locked memory rlimit.
 - ***`procstat.rlimit_memory_rss_hard`*** (*gauge*)<br>    The hard rss memory rlimit.
 - ***`procstat.rlimit_memory_rss_soft`*** (*gauge*)<br>    The soft rss memory rlimit.
 - ***`procstat.rlimit_memory_stack_hard`*** (*gauge*)<br>    The hard stack memory rlimit.
 - ***`procstat.rlimit_memory_stack_soft`*** (*gauge*)<br>    The soft stack memory rlimit.
 - ***`procstat.rlimit_memory_vms_hard`*** (*gauge*)<br>    The hard vms memory rlimit.
 - ***`procstat.rlimit_memory_vms_soft`*** (*gauge*)<br>    The soft vms memory rlimit.
 - ***`procstat.rlimit_nice_priority_hard`*** (*gauge*)<br>    The hard nice priority rlimit.
 - ***`procstat.rlimit_nice_priority_soft`*** (*gauge*)<br>    The soft nice priority rlimit.
 - ***`procstat.rlimit_num_fds_hard`*** (*gauge*)<br>    The hard file descriptor rlimit.
 - ***`procstat.rlimit_num_fds_soft`*** (*gauge*)<br>    The soft file descriptor rlimit.
 - ***`procstat.rlimit_realtime_priority_hard`*** (*gauge*)<br>    The hard realtime priority rlimit.
 - ***`procstat.rlimit_realtime_priority_soft`*** (*gauge*)<br>    The soft realtime priority rlimit.
 - ***`procstat.rlimit_signals_pending_hard`*** (*gauge*)<br>    The hard pending signal rlimit.
 - ***`procstat.rlimit_signals_pending_soft`*** (*gauge*)<br>    The soft pendidng signal rlimit.
 - ***`procstat.signals_pending`*** (*gauge*)<br>    The number of signals pending.
 - ***`procstat.write_bytes`*** (*gauge*)<br>    Number of bytes written by the process.  This may require the agent to be running as root.
 - ***`procstat.write_count`*** (*gauge*)<br>    Number of write operations by the process.  This may require the agent to be running as root.
 - ***`procstat_lookup.pid_count`*** (*gauge*)<br>    The number of pids. This metric emits with the plugin dimension set to "procstat_lookup".
The agent does not do any built-in filtering of metrics coming out of this
monitor.

