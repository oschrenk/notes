# Metrics #

[Metrics](https://github.com/codahale/metrics) Capturing JVM- and application-level metrics. So you know what's going on.


Metrics we can use 6 types of metrics:

- **Gauges**: an instantaneous measurement of a discrete value. 
- **Counters**: a value that can be incremented and decremented. Can be used in - queues to monitorize the remaining number of pending jobs.
- **Meters**: measure the rate of events over time. You can specify the rate unit, - the scope of events or  event type.
- **Histograms**: measure the statistical distribution of values in a stream of - data.
- **Timers**: measure the amount of time it takes to execute a piece of code and - the distribution of its duration.
- **Healthy checks**: as his name suggests, it centralize our service's healthy checks of external systems.