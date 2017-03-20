# Introduction
A simple &amp; reliable Shell script for dumping MySQL slowlog, especially friendly to [Logstash](https://www.elastic.co/products/logstash).
`slowlog_dump` tracks MySQL slowlogs, save them as they are under `<data_path>/slowlog.d/` and concatenate them to `<data_path>/slowlog.log`. Only _**delta**_ parts of slowlogs are dumped whenever `slowlog_dump` is run.

# Platform
* CentOS 6.6
* macOS 10.12
* Other Linux(NOT TESTED)

# Requirements
* `mysql` command
* `GRANT REPLICATION CLIENT ON *.* TO mysql_user_name@mysql.client.ip.or.domain;`
* `flock` command(best to have)
* `w(rite)` permission to `/var/lock/`

# Feature
* Simple: easy to deploy, only 1-2 script files, no need to run `yum install xxx`;
* Reliable: log rotation(`TRUNCATE mysql.slow_log` etc.) is supported by comparing MD5SUM of last local record with the remote one's;
* Incrementally Dumpping: only _**delta**_ parts of slowlogs are dumped whenever `slowlog_dump` is run. The whole of slowlog is dumped when log rotation is detected;
* Single Instance: at most 1 instance of this script is allowed to run;
* Friendly to Logstash: slowlogs are concatenated to `<data_path>/slowlog.log` which can be easily monitored by Logstash. `# EOF by 'slowlog_dump' script` line is appended to the end of `<data_path>/slowlog.log` which can be used to mark the start of next event by the `multiline` codec;

# Tutorial
```
# set `USER`, `PASSWD`, `HOST`, `PORT` variables
vim slowlog_dump

# create the <data_path> directory
mkdir data

# dump slowlogs
slowlog_dump data

# help
slowlog_dump

# structure of <data_path>
data
├── count_slowlog.all
├── error.log
├── mysql.task
├── slowlog.log
└── tmp.d
# count_slowlog.all: total number of local records and the content of last record
# error.log: running log
# mysql.task: [offset-rows count-rows] pair per line used by `mysql` 
# slowlog.log: concatenated slowlogs
# tmp.d: directory for temporary files
```
# Any Question
lslxdx#163.com
