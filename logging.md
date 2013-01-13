# Logging #

- usually stored as files
- although they are best represented as streams

- first class citizen
- always use [iso 8601](http://en.wikipedia.org/wiki/ISO_8601) for date formatting
- *Be professional*. Write in professional English.
- *Be concise*.
- *Be consistent*. Think about the message. Think about it hard. The message should never be changed in the future. It helps if you want to search for it in the future.
- no debugging logs in production
- do not log locally
- monitor your logs

## Log collector ##

- [fluentd](http://fluentd.org/). Ruby. APL2. Receives logs as JSON streams, buffers them, and sends them to other systems like Amazon S3, MongoDB, Hadoop, or other Fluentds.
- [scribe](https://github.com/facebook/scribe). C++. APL2. Server for aggregating log data that's streamed in real time from clients.
- [flume](http://flume.apache.org/) Java. APL2. Distributed, reliable, and available service for efficiently collecting, aggregating, and moving large amounts of log data.
