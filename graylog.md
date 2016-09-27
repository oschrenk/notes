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

```
sudo apt-get install apt-transport-https
wget https://packages.graylog2.org/repo/packages/graylog-2.1-repository_latest.deb
sudo dpkg -i graylog-2.1-repository_latest.deb
sudo apt-get update
sudo apt-get install graylog-server
```

Configure graylog

- set password secret
- set root password

Start graylog manually

```
sudo java -jar /usr/share/graylog-server/graylog.jar server
```

Configuring the webinterface is not the easiest thing

- http://docs.graylog.org/en/2.0/pages/configuration/web_interface.html#configuring-webif
- https://github.com/Graylog2/graylog2-server/issues/2179

## Testing inputs

Sending UDP gelf

```
echo -e '{"version": "1.1", "host": "staging", "_application": "myapp","_environment":"development","message":"Short message","level":1}\0' | nc -u -w 1 staging.acme.com 12201
```

- `\0` is needed
- underscore prefix is for custom fields
-  remove `-u` for netcat for tcp

