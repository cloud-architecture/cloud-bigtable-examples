# Bigtable Sample app packaging test

This is a sample app using the HBase native API to interact with Cloud Bigtable (anviltop).

## Install

You can Install the dependencies using maven.

First download the Cloud Bigtable client library and install it in your maven repository:

    $ gsutil -m cp -R gs://cloud-bigtable-eap .
    $ cd cloud-bigtable-eap/jars/current/
    $ mvn install:install-file -Dfile=bigtable-hbase-0.1.3.jar -DgroupId=com.google.bigtable.anviltop -DartifactId=bigtable-hbase -Dversion=0.1.3 -Dpackaging=jar -DgeneratePom=true

Then you can clone the repository and build the sample:

    $ git clone git@github.com:GoogleCloudPlatform/cloud-bigtable-examples.git
    $ cd cloud-bigtable-examples/java/simple-cli
    $ mvn install

## Provision a Bigtable Cluster

In order to provision a Cloud Bigtable cluster you will first need to create a Google Cloud Platform project. You can create a project using the [Developer Console](https://cloud.google.com/console).

After you have created a project you can create a new Cloud Bigtable cluster by clicking on the "Storage" -> "Cloud Bigtable" menu item and clicking on the "New Cluster" button.
After that, enter the cluster name, ID, zone, and number of nodes. Once you have entered those values, click the "Create" button to provision the cluster.

![New Cluster Form](../../../../blob/master/java/simple-cli/docs/new-cluster.png?raw=true)

TODO: add a link to docs.

## Set up your hbase-site.xml configuration

A sample hbase-site.xml is located in src/main/resources/hbase-site.xml.example. Copy it and enter the values for your project.

    $ src/main/resources
    $ cp hbase-site.xml.example hbase-site.xml
    $ vim hbase-site.xml

If one is not already created, you will need to [create a service account](https://developers.google.com/accounts/docs/OAuth2ServiceAccount#creatinganaccount)
and download the JSON key file.  After you have created the service account enter the project id and info for the service account in the locations shown.

    <configuration>
      <property>
        <name>hbase.client.connection.impl</name>
        <value>org.apache.hadoop.hbase.client.BigtableConnection</value>
      </property>
      <property>
        <name>google.bigtable.endpoint.host</name>
        <value>bigtable.googleapis.com</value>
      </property>
      <property>
        <name>google.bigtable.admin.endpoint.host</name>
        <value>table-admin-bigtable.googleapis.com</value>
      </property>
      <property>
        <name>google.bigtable.project.id</name>
        <value><!-- PROJECT ID --></value>
      </property>
      <property>
        <name>google.bigtable.cluster.name</name>
        <value><!-- BIGTABLE CLUSTER ID --></value>
      </property>
      <property>
        <name>google.bigtable.zone.name</name>
        <value><!-- ZONE WHERE CLUSTER IS PROVISIONED --></value>
      </property>
    </configuration>

## Run the code

Before running the application, make sure you have set the path to your JSON key file to the `GOOGLE_APPLICATION_CREDENTIALS` environment variable.

    $ export GOOGLE_APPLICATION_CREDENTIALS=/path/to/json-key-file.json

You can run a command using the hbasecli.sh script. The following command will get values from the row with key "row1" in the table "test".

    $ ./hbasecli.sh get test row1

## Contributing changes

* See [CONTRIBUTING.md](../..CONTRIBUTING.md)


## Licensing

* See [LICENSE](../../LICENSE)