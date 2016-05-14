#!/bin/sh
#
# american fuzzy lop - stop a set of fuzzers
# --------------------------------------
# 
# By MarkusTeufelberger, based on afl-whatsup, which is:
# Written and maintained by Michal Zalewski <lcamtuf@google.com>
#
# Copyright 2015 Google Inc. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at:
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# This stops all live fuzzers in the given output directory using
# SIGKILL

echo "afl-stop - stop fuzzers"
echo

DIR="$1"

if [ "$DIR" = "" ]; then

  echo "Usage: $0 afl_sync_dir" 1>&2
  echo 1>&2
  exit 1

fi

cd "$DIR" || exit 1

if [ -d queue ]; then

  echo "[-] Error: parameter is an individual output directory, not a sync dir." 1>&2
  exit 1

fi

TMP=`mktemp -t .afl-whatsup-XXXXXXXX` || exit 1

ALIVE_CNT=0
DEAD_CNT=0

for i in `find . -maxdepth 2 -iname fuzzer_stats`; do

  sed 's/^command_line.*$/_skip:1/;s/[ ]*:[ ]*/="/;s/$/"/' "$i" >"$TMP"
  . "$TMP"

  if ! kill -0 "$fuzzer_pid" 2>/dev/null; then
    echo "Instance is dead or running remotely, skipping."
    echo
    DEAD_CNT=$((DEAD_CNT + 1))
    continue
  fi

  ALIVE_CNT=$((ALIVE_CNT + 1))
  
  if ! kill -KILL "$fuzzer_pid" 2>/dev/null; then
    echo "Unable to stop fuzzer pid $fuzzer_pid, aborting."
    echo
    exit 1
  fi

done

rm -f "$TMP"

echo "Fuzzers stopped: $ALIVE_CNT"

if [ ! "$DEAD_CNT" = "0" ]; then
  echo "Dead or remote: $DEAD_CNT (excluded from stats)"
fi

exit 0
