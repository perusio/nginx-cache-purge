#!/bin/bash

# nginx-cache-purge --- A simple Bash script to delete an item or set
#                       of items specified through a grep pattern from
#                       the Nginx cache.

# Copyright (C) 2011 António P. P. Almeida <appa@perusio.net>

# Author: António P. P. Almeida <appa@perusio.net>

# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.

# Except as contained in this notice, the name(s) of the above copyright
# holders shall not be used in advertising or otherwise to promote the sale,
# use or other dealings in this Software without prior written authorization.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.

SCRIPTNAME=${0##*/}

function print_usage() {
    echo "$SCRIPTNAME <URI (grep pattern)> <cache directory>."
}

## Check the number of arguments.
if [ $# -ne 2 ]; then
    print_usage
    exit 1
fi

## Return the files where the items are cached.
## $1 - the filename, can be a pattern .
## $2 - the cache directory.
## $3 - (optional) the number of parallel processes to run for grep.
function get_cache_files() {
    ## The maximum number of parallel processes. 16 since the cache
    ## naming scheme is hex based.
    local max_parallel=${3-16}
    ## Get the cache files running grep in parallel for each top level
    ## cache dir.
    find $2 -maxdepth 1 -type d | xargs -P $max_parallel -n 1 grep -ERl "^KEY:.*$1" | sort -u
} # get_cache_files

## Removes an item from the given cache zone.
## $1 - the filename, can be a pattern .
## $2 - the cache directory.
function nginx_cache_purge_item() {
    local cache_files

    [ -d $2 ] || exit 2
    cache_files=$(get_cache_files "$1" $2)

    ## Act based on grep result.
    if [ -n "$cache_files" ]; then
        ## Loop over all matched items.
        for i in $cache_files; do
            [ -f $i ] || continue
            echo "Deleting $i from $2."
            rm $i
        done
    else
        echo "$1 is not cached."
        exit 3
    fi
} # nginx_cache_purge_item

## Try to purge the given item from the cache.
nginx_cache_purge_item $1 $2
