This contains two different versions of bro-cut, one with some known crashing bugs
and one that is fixed.

Usage
=====

    host$ docker build -t bro-cut-fuzz .
    host$ docker run -ti --rm bro-cut-fuzz
    container# afl-fuzz  -i input/ -o output/ -t 200 -- ./bro-cut-2.4.0

One should see crashes almost immediately
