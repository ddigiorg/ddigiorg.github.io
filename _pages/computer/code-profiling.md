---
layout: page
title: Code Profiling
date: 2021-11-22
tags: computer programming
---

## Optimizing

- <https://en.wikibooks.org/wiki/Optimizing_C%2B%2B>
- <https://www.slideshare.net/EmertxeSlides/embedded-c-optimization-techniques>


## Perf

Perf is a set of Linux tools for command profiling.

- Type `sudo pacman -S perf` to install the performance analysis tool for Linux
- Type `perf list` to list all currently known events
- Type `perf stat <command>` to get CPU counter statistics for the specified command
- Type `perf record <command>` to sample on-CPU functions for the specified command
- Type `perf report` to display perf.data (created by perf record)

Links:

- [perf wiki](https://perf.wiki.kernel.org/index.php/Main_Page)
- [perf examples](https://www.brendangregg.com/perf.html)

## Flame Graph

Flame graphs are visualizations of profiled software, allowing for the most frequence code paths to be identified quickly and accurately.

1. Type `git clone https://github.com/brendangregg/FlameGraph` to acquire FlameGraph tools
2. Type `perf record -F max -a -g -- ./<executable>` to capture executable
3. Type `perf script > out.perf` to read perf.data (created by perf record) and save trace output
4. Type `./FlameGraph/stackcollapse-perf.pl out.perf > out.folded` to fold stack samples into single lines
5. Type `./FlameGraph/flamegraph.pl out.folded > kernel.svg` to render a SVG
6. Type `brave kernel.svg` to view the SVG

Links:

- [Flame Graphs](http://www.brendangregg.com/flamegraphs.html)
- [FlameGraph git](https://github.com/brendangregg/FlameGraph)
