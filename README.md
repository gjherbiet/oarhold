oardelay
========

This script allows you to generate a lot of jobs using the `array-param-file`
option of `oarsub` but to execute few of them at a time.

Use this with a notify script that will *wake up* a new job a soon as one
is completed.

	./oardelay [NUMBER_OF_CONCURRENT_RUNNING_JOBS]

The default number of concurrent running jobs is set to 50.

oarwake
=======

This script *wakes up* a held job whose id is in a file when another job
completes.

To use with the `oardelay` script.

Configuration
=============

Set the following variables in `oardelay`:

	PROG="/path/to/myprogram"
	PROGNAME="somename"
	ARRAY_PARAM_FILE="./params.txt"
	NOTIFY_SCRIPT="/path/to/oarwake"
	JOBS_FILE="/path/to/jobs.txt"
	JOBS_LOCK_FILE="/path/to/jobs.lck"

	MAX_CONCURRENT_JOBS=50

Set the following variables in `oarwake`:

	JOBS_FILE="/path/to/jobs.txt"
	JOBS_LOCK_FILE="/path/to/jobs.lck"
	JOBS_LOG_FILE="/path/to/jobs.log"

Ensure that the `JOBS_FILE` and `JOBS_LOCK_FILE` are consistent between both scripts.

Usage
=====

After configuring both scripts, launch `./oardelay [N]` where N is the desired
number of concurrent running jobs.

It will create and hold as many instances of `PROG` required by the
`ARRAY_PARAM_FILE` content. Those jobs will only be scheduled and run `N` by `N`.

Each time a job completes, the `oarwake` script will be triggered, which will
schedule the next held job, until all held jobs have been executed.