#!/bin/bash

# Copyright (c) 2013-2014 Artem Shinkarov <artyom.shinkaroff@gmail.com>
#
# Permission to use, copy, modify, and distribute this software for any
# purpose with or without fee is hereby granted, provided that the above
# copyright notice and this permission notice appear in all copies.
#
# THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES
# WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR
# ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES
# WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN
# ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF
# OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

set -e

script_name=$(basename $0)
params=$(getopt -o vnbdup:h -l version,nofork,beep,dpms,no-unlock-indicator,pointer:,help -n "$script_name" -- "$@")
eval set -- "$params"

# exit on bad argyments
if [ $? -ne 0 ]; then
    exit 1
fi

usage () {
    cat <<EOF
A wrapper around i3lock that sets a blurred screenshot 
as a background image for the screen saver.

Usage: $script_name [options]

    -v, --version               version of the i3lock.
    -n, --nofork                don't fork i3lock after start.
    -b, --beep                  enable beeping.
    -d, --dpms                  enable turning off the screen with DPMS.
    -u, --no-unlock-indicator   disable the unlock indicator.
    -p win|default, 
    --pointer=win|default       do not hide the mouse pointer on default
                                or the current position of the mouse pointer.

    Please note, that all the options are being passed directly to i3lock.
    See more details via man i3lock.
EOF
}

arg_test () { 
    # Filter out arguments that are invalid
    while true; do
       case "$1" in
           -v|--version)
               i3lock -v
               exit
               ;;
           -n|--nofork)                
               shift;;
           -b|--beep)              
               shift;;
           -d|--dpms)                  
               shift;;
           -u|--no-unlock-indicator)   
               shift;;
           -p|--pointer) 
               shift
               case "$1" in
                   win|default)
                       shift;;
                   *)
                       echo "$script_name: invalid option \"$1\" for argument -p" >&2
                       exit 1
                       ;;
               esac
               ;;
           -h|--help)
               usage
               exit
               ;;
           --)
               shift 
               break ;;

           *)
               echo "$script_name: invalid argument \"$1\"" >&2
               exit 1
               ;;
       esac
    done

    if [ $# -ne 0 ]; then
       echo "$script_name: invalid arguments \"$@\""
       exit 1
    fi
}

arg_test $@

file1=$(mktemp --tmpdir i3lock-wrapper-XXXXXXXXXX.png)
file2=$(mktemp --tmpdir i3lock-wrapper-XXXXXXXXXX.png)

scrot -d0 "$file1"
convert "$file1" -blur 0x3 "$file2"

# drop the last "--" introduced by getopt
i3lock -i "$file2" ${@:1:($#-1)}

trap "rm '$file1' '$file2'" EXIT
