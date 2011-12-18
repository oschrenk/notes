# Unix Tools #

## pv ##

Found on [Peteris Krumins' blog ](http://www.catonmat.net/blog/unix-utilities-pipe-viewer/)

`pv` - Pipe Viewer - is a terminal-based tool for monitoring the progress of data through a pipeline. It can be inserted into any normal pipeline between two processes to give a visual indication of how quickly data is passing through, how long it has taken, how near to completion it is, and an estimate of how long it will be until completion.

### Installation ###

	$ brew install pv # OSX
	$ apt-get install pv # Debian

### Usage ###

Suppose you want to compress a logfile

	$ gzip -c access.log > access.log.gz

The file is so huge (several gigabytes), you have no idea how long to wait. Will it finish soon? Or will it take another 30 mins?

Using `pv` you can precisely time how long it will take

	$ pv access.log | gzip > access.log.gz
	611MB 0:00:11 [58.3MB/s] [=>      ] 15% ETA 0:00:59

Pipe viewer acts as `cat` here, except it also adds a progress bar. We can see that gzip processed 611MB of data in 11 seconds. It has processed 15% of all data and it will take 59 more seconds to finish.

You may stick several pv processes in between. For example, you can time how fast the data is being read from the disk and how much data is gzip outputting:

	$ pv -cN source access.log | gzip | pv -cN gzip > access.log.gz
	source:  760MB 0:00:15 [37.4MB/s] [=>     ] 19% ETA 0:01:02
	  gzip: 34.5MB 0:00:15 [1.74MB/s] [  <=>  ]

Here we specified the "-N" parameter to pv to create a named stream. The `-c` parameter makes sure the output is not garbaged by one pv process writing over the other.