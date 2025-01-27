# Telemetry

The Sherpa server collects various runtime metrics about the performance that are retained for one minute. This data can be viewed either by sending the Sherpa server process a signal, or [configuring](./README.md) the server to stream data to [statsite](https://github.com/statsite/statsite) or [statsd](https://github.com/statsd/statsd).

To view this data via sending a signal to the Sherpa process: on Unix, this is `USR1` while on Windows it is `BREAK`. Once Nomad receives the signal, it will dump the current telemetry information to the server's `stderr`:

```bash
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.sys_bytes': 72220920.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.malloc_count': 76736.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.free_count': 41066.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.heap_objects': 35670.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.total_gc_pause_ns': 39109.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.total_gc_runs': 1.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.num_goroutines': 7.000
[2019-05-14 18:32:50 +0100 BST][G] 'sherpa.lluna.local.runtime.alloc_bytes': 3044160.000
[2019-05-14 18:32:50 +0100 BST][S] 'sherpa.runtime.gc_pause_ns': Count: 1 Sum: 39109.000 LastUpdated: 2019-05-14 18:32:54.504907 +0100 BST m=+1.125110442
```

# Runtime Metrics

Runtime metrics allow operators to get insight into how the Sherpa server process is functioning.

<table class="table table-bordered table-striped">
  <tr>
    <th>Metric</th>
    <th>Description</th>
    <th>Unit</th>
    <th>Type</th>
  </tr>
  <tr>
    <td>`sherpa.runtime.num_goroutines`</td>
    <td>Number of goroutines and general load pressure indicator</td>
    <td>Number of goroutines</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.alloc_bytes`</td>
    <td>Number of bytes allocated to the Sherpa process which should keep a steady state</td>
    <td>Number of bytes</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.sys_bytes`</td>
    <td>This includes what is being used by Sherpa's heap and what has been reclaimed but not given back to the operating system</td>
    <td>Number of bytes</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.malloc_count`</td>
    <td>Cumulative count of allocated heap objects</td>
    <td>Number of heap objects</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.free_count`</td>
    <td>Number of freed objects from the heap and should steadily increase over time</td>
    <td>Number of freed objects</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.heap_objects`</td>
    <td>This is a good general memory pressure indicator worth establishing a baseline and thresholds for alerting</td>
    <td>Number of objects in the heap</td>
    <td>Gauge</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.total_gc_pause_ns`</td>
    <td>The total garbage collector pause time since Sherpa was last started</td>
    <td>Milliseconds</td>
    <td>Summary</td>
  </tr>
  <tr>
    <td>`sherpa.runtime.total_gc_runs`</td>
    <td>Total number of garbage collection runs since Sherpa was last started</td>
    <td>Number of operations</td>
    <td>Gauge</td>
  </tr>
</table>
