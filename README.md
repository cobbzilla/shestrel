shestrel
========

A set of simple shell scripts for working with kestrel queues. Commands supported are:

* kget: read from a queue
* kset: write to a queue
* ksize: show queue size
* kflush: flush a queue
* kflush_all: flush all queues
* kmove: move all items from one queue to another queue
* kpopulate: write many items to a queue

## Referencing a queue
For all commands (except kflush_all), the format is

    [host[:port]/]queue
where:

* host is optional and will default to localhost
* port is optional and will default to 22133
* queue is required

For kflush_all, the format is

    host[:port]
where:
* port is optional and will default to 22133
* host is required

## Exit codes
* Commands exit with status 0 upon success. 
* Exit status 1 indicates a problem with the command arguments.
* kget exits with status 2 if the queue is empty.

## Examples
    $ ksize foo
    0
    $ kpopulate foo 100
    $ ksize foo
    100
    $ kflush foo
    $ ksize foo
    0
    $ kpopulate foo 23
    $ kget foo
    value_0
    $ ksize foo
    22
    $ kset foo another_value
    $ ksize foo
    23
    $ kmove foo bar
    Moved 23 items
    $ ksize foo
    0
    $ ksize bar
    23
    $ kpopulate foo 100
    $ ksize foo
    100
    $ ksize bar
    23
    $ kflush_all
    Flushed all queues.
    $ ksize foo
    0
    $ ksize bar
    0
    $ kset localhost:22133/foo some_value
    $ kget localhost:22133/foo
    some_value
    $ kset localhost/foo one_more_value
    $ kget localhost/foo
    one_more_value
