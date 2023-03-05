# Perf

- Great resource for perf in general: [https://www.brendangregg.com/perf.html](https://www.brendangregg.com/perf.html)
- Unofficial linux perf events page: [https://web.eece.maine.edu/~vweaver/projects/perf_events/](https://web.eece.maine.edu/~vweaver/projects/perf_events/)
- Perf Timechart manpage: [https://manpages.ubuntu.com/manpages/impish/man1/perf-timechart.1.html](https://manpages.ubuntu.com/manpages/impish/man1/perf-timechart.1.html)

## Visualizers
- Flamegraph:
  - [https://github.com/brendangregg/FlameGraph](https://github.com/brendangregg/FlameGraph)
  - [https://www.brendangregg.com/flamegraphs.html](https://www.brendangregg.com/flamegraphs.html)
- Speedscope visualizer:
  - [https://www.speedscope.app](https://www.speedscope.app)
  - [https://github.com/jlfwong/speedscope](https://github.com/jlfwong/speedscope)
- Hotspot:
  - [https://github.com/KDAB/hotspot](https://github.com/KDAB/hotspot)
- Firefox Profiler:
  - [https://profiler.firefox.com](https://profiler.firefox.com)

Install (Debian): `apt install linux-tools-<kernel-version> linux-perf-<kernel-version>`

Compile applications with: `-fno-omit-frame-pointer`

```bash
# Recording Examples

# All processes with simple callstack for 30 seconds.
perf record -ag -- sleep 30

# 99 samples per second.
perf record -F 99 -ag -- sleep 30

# Specific process with simple callstack for 60 seconds.
Perf record -g -p <pid> -- sleep 60

# Specific process with dwarf-callstack
perf record --call-graph=dwarf -p <pid> -- sleep 60
perf record -F 99 --call-graph=dwarf -p <pid> -- sleep 60
```

```bash
# For firefox visualizer
perf record -a -g -F 1000 > perf.data
perf script -F +pid | gzip > perf.gz
```

```bash
# Analysis

# Output as Tree
perf report -n --stdio
perf report --call-graph --stdio -G

# Output as flamegraph
# http://www.brendangregg.com/FlameGraphs/cpuflamegraphs.html
# Flamegraphs need `flamegraph.pl` and `stackcollapse-perf.pl` from https://github.com/brendangregg/FlameGraph

perf script | ./stackcollapse-perf.pl > out.perf-folded
./flamegraph.pl out.perf-folded > perf-kernel.svg
```

```bash
# perf timechart can also be used to see the timing of processes better

# Records
perf timechart record

# Makes SVG
# WILL BE VERY BIG! TRY INKSCAPE OR EVEN BETTER ADOBE ILLUSTRATOR TO VIEW!
perf timechart
```

```bash
# Compiler Flags:

V1:
-g -fno-inline -fno-omit-frame-pointer -fno-optimize-sibling-calls -marm

V2:
-g -fno-inline -fno-omit-frame-pointer -marm -mapcs-frame

V3:
-g -fno-inline -fno-omit-frame-pointer -fno-optimize-sibling-calls -marm -mapcs-frame
```