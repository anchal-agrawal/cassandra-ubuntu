# cassandra-ubuntu

Everything to deploy a multi-node, single-datacenter Cassandra cluster on Ubuntu.

This guide works for Cassandra versions `3.x`. Steps for future versions might differ.

#### Bootstrap node

* `sudo apt-get update`
* Install Oracle Java 8 or OpenJDK 8. [See this Stackoverflow answer.](http://stackoverflow.com/a/33932047)

#### Install Cassandra as a service

* `echo "deb http://debian.datastax.com/datastax-ddc 3.2 main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list` 
   (replace `3.2` with the version you're installing)
* `curl -L https://debian.datastax.com/debian/repo_key | sudo apt-key add -`
* `sudo apt-get update`
* `sudo apt-get install datastax-ddc`

Pick certain nodes as [seed nodes](http://wiki.apache.org/cassandra/FAQ#seed). It is recommended to have more than one seed per cluster for fault tolerance.

#### Initialize the cluster

* First, stop Cassandra and clear data since the package installation starts it automatically.
  * `sudo service cassandra stop`
  * `sudo rm -rf /var/lib/cassandra/data/system/*`
* Edit `/etc/cassandra/cassandra.yaml`. See [cassandra.yaml](https://github.com/anchal-agrawal/cassandra-ubuntu/blob/master/cassandra.yaml). For advanced properties, see [this](http://docs.datastax.com/en/cassandra/3.x/cassandra/initialize/initSingleDS.html).

#### Start Cassandra

* First, start all seeds and then the other nodes.
  * `sudo service cassandra start`
* Check that the service is running.
  * `sudo service cassandra status`
* Check that all nodes are part of the cluster.
  * `sudo nodetool status`
* Use the `cqlsh` utility to operate the cluster.

#### References

1. http://docs.datastax.com/en/cassandra/3.x/cassandra/install/installDeb.html
2. http://docs.datastax.com/en/cassandra/3.x/cassandra/initialize/initSingleDS.html
3. http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureFireWall_r.html

#### Troubleshooting

1. Startup failures? Logs are your best friend! `/var/log/cassandra/`
2. http://docs.datastax.com/en/cassandra/3.x/cassandra/troubleshooting/trblshootTOC_g.html
