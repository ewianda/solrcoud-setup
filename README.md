# solrcoud-setup

wget http://archive.apache.org/dist/lucene/solr/5.5.3/solr-5.5.3.tgz
tar xzf solr-5.5.3.tgz  solr-5.5.3/bin/install_solr_service.sh --strip-components=2
sudo su
./install_solr_service.sh solr-5.5.3.tgz 
sudo service solr stop
edit /etc/default/solr.in.sh

#SOLR_JAVA_MEM="-Xms512m -Xmx512m"
#SOLR_HEAP="512m"

SOLR_JAVA_MEM="-Xms10g -Xmx10g"

/opt/solr/bin/solr start -cloud -z 10.1.0.8:2181

 /opt/solr/bin/solr create_collection -shards 4  -replicationFactor 2 -n sku-config -c sku
 
 sudo su - solr -c '/opt/solr/bin/solr create -c protein -d /var/solr/data/default_conf'
 

