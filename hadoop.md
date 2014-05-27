# Hadoop #

	brew install hadoop

## Configure ##

	cd $(brew --prefix)/Cellar/hadoop/$(brew info hadoop | grep "hadoop:" | grep -o "[0-9].[0-9].[0-9]")/libexec

Add the following lines to `conf/core-site.xml` inside the configuration tags:

	<property>
		<name>fs.default.name</name>
		<value>hdfs://localhost:9000</value>
	</property>

Add the following lines to `conf/hdfs-site.xml` inside the configuration tags:

	<property>
		<name>dfs.replication</name>
		<value>1</value>
	</property>

Add the following lines to `conf/mapred-site.xml` inside the configuration
tags:

	<property>
		<name>mapred.job.tracker</name>
		<value>localhost:9001</value>
	</property>

Format the filesystem

	bin/hadoop namenode -format

Enable Remote Login

	sudo systemsetup -setremotelogin on

