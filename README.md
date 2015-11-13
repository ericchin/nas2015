# Liferay NAS 2015 - ELK: Unlocking Your Data's Potential

## Apache (for Liferay)
Add the below in the `log_config_module` section with the other log configurations. This is the format that the logstash apache configuration file will parse.

`LogFormat "%h %l %u %t %m %f \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" \"%U\"  \"%q\" %T %D" liferay`

## Elasticsearch
**Download link**: [https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)

Elasticsearch can be started out of the box. There is no configuration necessary, unless the default functionality of elasticsearch needs to be configured. To start elasticsearch, run the following command:
`./bin/elasticsearch`

### Demo Setup
This configuration consists of 3 elasticsearch servers. 1 server acts as a load balancer (client node). The other 2 servers act as data nodes that carry the indices.

See the elasticsearch configuration files in the elasticsearch folder of this repository. The port numbers and the configuration file directories were set through environment variables before starting the servers.

This is an example of the environment variable:
`export ES_JAVA_OPTS='-Dcluster.name=elasticsearch -Des.transport.tcp.port=7300 -Des.config=/opt/app/es1/config/elasticsearch.yml'`

For more information about elasticsearch clustering, please refer to the page here: [https://www.elastic.co/guide/en/kibana/current/production.html#load-balancing](https://www.elastic.co/guide/en/kibana/current/production.html#load-balancing)

#### Client Node
The client node acts as a load balancer. It is not the master node and does not hold any data.

The configuration below configures this elasticsearch node as the client node (load balancer).

```
node.master: false 
node.data: false
```

#### Data Nodes
These are the nodes that actually hold the data. The client node will direct all the requests to any data node in the cluster. The default settings will work fine.

## Logstash
**Download link**: [https://www.elastic.co/downloads/logstash](https://www.elastic.co/downloads/logstash)

**Starting Logstash**

1. Copy the conf files in the respective folders of the logstash folder over to your logstash folder. 
2. Run the logstash command below from the logstash home directory. (The same logstash command can be run for each configuration file.)

`./bin/logstash agent -f logstash.conf`

The patterns directory contains the patterns that are referenced from the logstash configuration files. Place these files in the patterns directory.

In the configuration, the patterns directory is set to `/opt/app/kibana/patterns`. [https://github.com/ericchin/nas2015/blob/master/logstash/patterns/apache.txt](https://github.com/ericchin/nas2015/blob/master/logstash/patterns/apache.txt) and [https://github.com/ericchin/nas2015/blob/master/logstash/patterns/liferay.txt](https://github.com/ericchin/nas2015/blob/master/logstash/patterns/liferay.txt) should be placed in the patterns directory that is set in the logstash configuration files.

## Kibana
**Download link**: [https://www.elastic.co/downloads/kibana](https://www.elastic.co/downloads/kibana)

Unpack the Kibana archive and start Kibana by running `./bin/kibana` from the Kibana home folder. Kibana can be accessed from the browser on port 5601: [http://localhost:5601](http://localhost:5601).

The elasticsearch url can be configured in `{KIBANA_HOME}/config/kibana.yml`. Modify the value of the `elasticsearch_url` parameter to set the location of the elasticsearch server.