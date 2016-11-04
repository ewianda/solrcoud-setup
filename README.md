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
 

 wget http://search.maven.org/remotecontent?filepath=org/opensextant/solr-text-tagger/2.3/solr-text-tagger-2.3.jar 
mkdir /var/solr/data/lib
 cp remotecontent\?filepath\=org%2Fopensextant%2Fsolr-text-tagger%2F2.3%2Fsolr-text-tagger-2.3.jar  /var/solr/data/lib/solr-text-tagger-2.3.jar 
 
 
   <codecFactory name="CodecFactory" class="solr.SchemaCodecFactory" />
   
  <query>
    <!-- illustrate putting in memory for warm-up -->
    <listener event="firstSearcher" class="solr.QuerySenderListener">
      <arr name="queries">
        <lst><str name="q">name_tag:[* TO *]</str></lst>
      </arr>
    </listener>
    <listener event="newSearcher" class="solr.QuerySenderListener">
      <arr name="queries">
        <lst><str name="q">name_tag:[* TO *]</str></lst>
      </arr>
    </listener>
  </query>

  <requestHandler name="/select" class="solr.SearchHandler"></requestHandler>

  <requestHandler name="/tag" class="org.opensextant.solrtexttagger.TaggerRequestHandler">
    <lst name="defaults">
      <str name="field">name_tag</str>
      <str name="fq">NOT name:(of the)</str><!-- filter out -->
    </lst>
  </requestHandler>

  <requestHandler name="/tagStop" class="org.opensextant.solrtexttagger.TaggerRequestHandler">
    <!-- top level params; legacy format just to test it still works -->
    <str name="field">name_tagStop</str>
  </requestHandler>

  <requestHandler name="/tagPartial" class="org.opensextant.solrtexttagger.TaggerRequestHandler">
    <!-- top level params; legacy format just to test it still works -->
    <str name="field">name_tagPartial</str>
    <str name="fq">NOT name:(of the)</str><!-- filter out -->
  </requestHandler>

  <requestHandler name="/tagXml" class="org.opensextant.solrtexttagger.TaggerRequestHandler">
    <!-- top level params; legacy format just to test it still works -->
    <str name="field">name_tagXml</str>
  </requestHandler>
  
  
  
