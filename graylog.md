# Graylog

Install dependencies

    apt-get install apt-transport-https uuid-runtime pwgen

Install elasticsearch

	wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
	echo "deb https://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
	sudo apt-get update && sudo apt-get install elasticsearch

Configure elasticsearch

Make sure to modify the Elasticsearch configuration file (`/etc/elasticsearch/elasticsearch.yml`) and set the cluster name to graylog:

	cluster.name: graylog

!!! Other installation guides are talking about shutting down external access. We need to look into this

Restart

	sudo service elasticsearch restart


Install graylog

  wget https://packages.graylog2.org/repo/packages/graylog-2.0-repository_latest.deb
  sudo dpkg -i graylog-2.0-repository_latest.deb
  sudo apt-get update && sudo apt-get install graylog-server

Configure graylog

    set secret hash and pass hash
	  password: sunny-dolphin-lad-500

	  Change elastic search shards to 1

Start graylog manually

    sudo java -jar /usr/share/graylog-server/graylog.jar server
