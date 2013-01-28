# Metrics, Metrics everywhere #

Notes taken from [Coda Hale](http://codahale.com/)'s presentation called [Metrics, Metrics everywhere](http://www.youtube.com/watch?v=czes-oa0yik) at CodeConf 2011

You write code that generates business value. But what is business value?

- A new feature
- An improved old feature
- Fixing a bug
- Make it faster, prettier, easier
- Add unit test before a bug

We need to make better decision about our code.

- Our code generates business value when it runs, not when we write it
- What does our code do, when we run it
- We can only see what it does it when we run it

Why measure it?

- Map /= territory
- Perception /= reality
- What we know /= What actually is

We have a mental model what our code does

- Model is often wrong
- That generates confusion

Seeing this confusion/gap is hard. We don't have a physical mapping

Measuring gives us

- a better mental model
- a better mapping from map to territory

But only if we're measuring the right thing!

- measure code it in the wild, in production!

Introducing Metrics [Java](https://github.com/codahale/metrics) Library

- **Gauge** Instantaneous value of something (`# of cities`)
- **Counters** Incrementing and decrementing values  (`# of open connections`)
- **Meters** Average rate of events over a period of time (`# of requests per sec`)
- **Histogram** The statistical distribution of values in a stream of data (minimum, maximum, mean, standard deviation)
- **Timers** A histogram of duration and a meter of calls (`# of ms to respond`)

Your seldom interested in the mean rate. You normally want recent values or quantiles (median 75th, 95th percentile, 98percentile, 99 percentile)

Techniques:

- Exponentially weighted moving average
- **Reservoir sampling**. Keep a statistically representative sample of measurements as they happen. Vitter's algorithm R. It produces uniform samples.
- **Forward decaying priority sampling**. It allows to maintain a statistically representation representative sample if the last 5 minutes

If it could affect your business value, measure it.

- Collect the data
- Expose the data
- Collect it
- Monitor it.

If it affects your business values someone gets mailed/woken up

Aggregate it using servives like
- [Ganglia](http://ganglia.sourceforge.net/)
- [Graphite](http://graphite.wikidot.com/)
- [Cacti](http://www.cacti.net/)

Doing so allows you to

- Place current values in historical context.
- See long term.

Shorten decision making cycle.

- **O**bserve. *Q*: What is the 99% latency of our autocomplete service right now? *A*: ~500ms
- **O**rient. *Q*: How does this compare to other parts of our system, both currently and historically? *A*: way slower!
- **D**ecide. *Q*: Should we make it faster? Or should we add feature X? *A*: Make it faster.
- **A**ct. Write code!

Summary
- In order to know how well our code is generating business value, we need metrics.
- Monitor it.
- Aggregate it to see historical changes.
- Better model.

- Metrics, [Javascript](https://github.com/mikejihbe/metrics)
- Metrics, [Java](https://github.com/codahale/metrics)
- Metrics, [Ruby](https://github.com/johnewart/ruby-metrics)
