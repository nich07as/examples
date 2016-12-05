### CEF Demo
This **Getting Started with Elastic Stack** example provides sample files to ingest, analyze & visualize **CEF data** using the Elastic Stack, i.e. Elasticsearch, Logstash and Kibana.
The sample files in this example are in the CEF format.  The example provided illustrates indexing manually through the Logstash TCP input.  This same configuration can be used to ingest events from Arcsight as described [here](Blog Link)[]: 


##### Version
Example has been tested in following versions:

- Elasticsearch 5.0
- X-Pack 5.0
- Logstash 5.0
- Kibana 5.0
- Arcsight (Optional)

### Installation & Setup
* Follow the [Installation & Setup Guide](https://github.com/elastic/examples/blob/master/Installation%20and%20Setup.md) to install and test the Elastic Stack (*you can skip this step if you have a working installation of the Elastic Stack,*)

* Run Elasticsearch & Kibana
  ```shell
    <path_to_elasticsearch_root_dir>/bin/elasticsearch
    <path_to_kibana_root_dir>/bin/kibana
    ```

* Check that Elasticsearch and Kibana are up and running.
  - Open `localhost:9200` in web browser -- should return status code 200
  - Open `localhost:5601` in web browser -- should display Kibana UI.

  **Note:** By default, Elasticsearch runs on port 9200, and Kibana run on ports 5601. If you changed the default ports, change   the above calls to use appropriate ports.

### Download Example Files

Download the following files in this repo to a local directory:
- `cef_data` - sample data (in cef format)
- `cef_logstash.conf` - Logstash config for ingesting data into Elasticsearch
- `cef_template.json` - template for custom mapping of fields
- `cef_kibana.json` - config file to load prebuilt creating Kibana dashboard

The following additional files are optional:
 
- `cef_dashboard.png` - screenshot of final Kibana dashboard  

Unfortunately, Github does not provide a convenient one-click option to download entire contents of a subfolder in a repo. Use sample code provided below to download the required files to a local directory:

```shell
mkdir  CEF_Example
cd CEF_Example
wget https://raw.githubusercontent.com/elastic/examples/master/Security_Analytics/cef_demo/cef_logstash.conf
wget https://raw.githubusercontent.com/elastic/examples/master/Security_Analytics/cef_demo/cef_template.json
wget https://raw.githubusercontent.com/elastic/examples/master/ElasticStack_apache/cef_demo/cef_data
wget https://raw.githubusercontent.com/elastic/examples/master/Security_Analytics/cef_demo/cef_kibana.json
```

### Run Example
##### 1. Ingest data into Elasticsearch using Logstash
* Execute the following command to load sample cef data into Elasticsearch. [Note: It takes a few minutes to ingest the entire file (~300,000 logs) into Elasticsearch]

```shell
cat cef_logs/* | nc localhost 5000 | <path_to_logstash_root_dir>/bin/logstash -f cef_logstash.conf
```

 * Verify that data is successfully indexed into Elasticsearch

  Running `http://localhost:9200/cef-*/_count` should return a response a `"count":TODO`

 **Note:** Included `cef_logstash.conf` configuration file assumes that you are running Elasticsearch on the same host as Logstash and have not changed the defaults. Modify the `host` and `cluster` settings in the `output { elasticsearch { ... } }`   section of cef_logstash.conf, if needed.

##### 2. Visualize data in Kibana

* Access Kibana by going to `http://localhost:5601` in a web browser
* Connect Kibana to the `cef` indices in Elasticsearch (autocreated in step 1)
    * Click the **Management** tab >> **Index Patterns** tab >> **Create New**. Specify `cef-*` as the index pattern name and click **Create** to define the index pattern. (Leave the **Use event times to create index names** box unchecked and use @timestamp as the Time Field)
* Load sample dashboard into Kibana
    * Click the **Management** tab >> **Saved Objects** tab >> **Import**, and select `cef_kibana.json`
* Open dashboard
    * Click on **Dashboard** tab and open `Sample Dashboard for CEF Logs` dashboard

Voila! You should see the following dashboard. Happy Data Exploration!

![Kibana Dashboard Screenshot](TODO)

### We would love your feedback!
If you found this example helpful and would like more such Getting Started examples for other standard formats, we would love to hear from you. If you would like to contribute Getting Started examples to this repo, we'd love that too!
