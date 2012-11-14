# ZeroMQ #

I have a multi account system, so I have to switch to my administrator account.
	
	su admin 

Install [∅mq](http://www.zeromq.org/) form source (the latest homebrew version `3.2.1-rc2` had some errors that cause jzmq unit tests to fail)

	cd ~/Projects/external
	git clone git://github.com/zeromq/libzmq.git
	cd libzmq
	./autogen.sh
	./configure
	make
	make install

Exit to the normal user

	exit

## jzmq ##

Switch to admun user

	su admin

Install latest [jzmq](https://github.com/zeromq/jzmq)

	cd ~/Projects/external
	git clone git://github.com/zeromq/jzmq.git
	cd jzmq
	./autogen.sh
	./configure
	make
	make install

Test the installation

	cd perf
	java -Djava.library.path=/usr/local/lib -classpath /usr/local/share/java/zmq.jar:../src/zmq.jar:zmq-perf.jar local_lat tcp://127.0.0.1:5555 1 100

At the first run this fails

	...
	Library not loaded: /usr/local/lib/libzmq.1.dylib
	  Referenced from: /usr/local/lib/libjzmq.0.dylib

And indeed the file does not exist, but `libzmq.3.dylib` does exist, so

	ln -s /usr/local/lib/libzmq.3.dylib /usr/local/lib/libzmq.1.dylib

Rerun the perf test
	
	java -Djava.library.path=/usr/local/lib -classpath /usr/local/share/java/zmq.jar:../src/zmq.jar:zmq-perf.jar local_lat tcp://127.0.0.1:5555 1 100
	java -Djava.library.path=/usr/local/lib -classpath /usr/local/share/java/zmq.jar:../src/zmq.jar:zmq-perf.jar remote_lat tcp://127.0.0.1:5555 1 100

Proceed to compile the maven artifact

	cd ..
	export JAVA_HOME=`/usr/libexec/java_home -v 1.7`
	mvn clean install -DskipTests

Exit to the normal user

	exit