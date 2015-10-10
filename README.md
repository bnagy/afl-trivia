afl-trivia
=======

## About

A small collection of scripts that were once gists.

### `afl-pause` & `afl-resume`

Pause and resume a set of running fuzzers using SIGSTOP / SIGCONT.

### `afl-consolidate`

Consolidate and de-dup all queue and crash files from a set of fuzzers.

### `afl-pollenate`

Pollenate a sync directory between groups of fuzzers running against different
targets. Useful when you are fuzzing eg three different PDF rendering engines.

### `afl-pcmin`

Small modifications to `afl-cmin` to use the GNU parallel tool. Parallelises
the initial tracing and some of the sorting. Also supports clobbering an
existing output directory.

## TODO

- Work out how to parallelise the final selection phase (step 5) in afl-pcmin

## Contributing

* Fork and send a pull request
* Report issues

## License & Acknowledgements

 `afl-consolidate` and `afl-pollenate` are released under a permissive but 
 non-GPL compatible license (based on the 4-clause BSD license). See 
 LICENSE file for details. I'm not a fan of the GPL.

The other tools are modified versions from the afl source, so they remain 
(c) Google Inc and are licensed under the Apache License 2.0.

