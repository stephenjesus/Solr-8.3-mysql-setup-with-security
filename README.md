# Solr-8.3-mysql-setup-with-security





1.Download latest 6.x Solr release from http://lucene.apache.org/solr/


2.Open the folder and start Solr instance and create your first core


./bin/solr start
./bin/solr create -c products



3.Open http://localhost:8983/solr/ and check that collection has been created.



4. Go to ./server/solr/products/conf/solrconfig.xml and edit solrconfig.xml, add the following before existing <lib… statements:

<lib dir="${solr.install.dir:../../../..}/contrib/dataimporthandler/lib" regex=".*\.jar" />
<lib dir="${solr.install.dir:../../../..}/dist/" regex="solr-dataimporthandler-.*\.jar" />



And the following to section <!– Request Handlers

<requestHandler name="/dataimport" class="org.apache.solr.handler.dataimport.DataImportHandler">
<lst name="defaults">
<str name="config">solr-data-config.xml</str>
</lst>
</requestHandler>

Save and close the file.


5. Create a file called ./server/solr/products/conf/solr-data-config.xml with the following contents

examples : -
<dataConfig>
<dataSource type="JdbcDataSource"
driver="com.mysql.jdbc.Driver"
url="jdbc:mysql://localhost:3306/db_name"
user="root"
password="password"/>
<document>
<entity name="entity_name"
query="SELECT * FROM TABLE_NAME">
<field column="id" name="id"/>
<field column="name" name="name"/>
</entity>
</document>
</dataConfig>

<!-- mysql -h192.168.86.109 -uroot -pExamly@123$ -->
database name - exams

<dataConfig>
<dataSource type="JdbcDataSource"
driver="com.mysql.jdbc.Driver"
url="jdbc:mysql://localhost:3306/exams"
user="root"
password="Examly@123$"/>
<document>
<entity name="marketplace"
query="SELECT * FROM products">
<field column="id" name="id"/>
<field column="title" name="title"/>
<field column="price" name="price"/>
</entity>
</document>
</dataConfig>


change managed schema also



    <field name="id" type="string" indexed="true" stored="true" required="true" multiValued="false" />
    <field name="title" type="string" stored="true" required="true" multiValued="false" />
    <field name="status" type="string" stored="true" required="true" multiValued="false" />
    <field name="entity_type" type="string" stored="true" required="true" multiValued="false" />
  


    <field name="ratings" type="pfloats" stored="true" required="true" multiValued="false" />
    <field name="price" type="pfloats" stored="true" required="true" multiValued="false" />    
    <field name="review_count" type="pints" stored="true" required="true" multiValued="false" />
    <field name="product_duration" type="pints" stored="true" required="true" multiValued="false" />

    
   <field name="level" type="string" stored="true" required="false" multiValued="false" />
   <field name="topic_ids" type="_nest_path_" stored="true" required="false" multiValued="true" />

Make sure you replace username and password, and corresponding query for the entity. Describe each field as needed.


Connect with Mysql and restart Solr

6. You will need to add the following. 

Download JDBC driver for MySQL from http://dev.mysql.com/downloads/connector/j/.

Copy file from the downloaded archive ‘mysql-connector-java-*.jar’ to the folder ‘./contrib/dataimporthandler/lib’ in the folder where Solr was installed. Create ‘lib’ folder if needed.


7. Restart Solr

./bin/solr restart

8. Go to http://localhost:8983/solr/, find your collection and add your fields as needed to the schema (select Schema in collection menu).

9. Do a full import by selecting Dataimport from the menu, execute it.
