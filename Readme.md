# Overview
This is fun project to explore ELK capabilities for Stock analysis. Here we monitor bhavcopy file downloaded from https://www1.nseindia.com/products/content/all_daily_reports.htmhttps://www1.nseindia.com/products/content/all_daily_reports.htm
Logstash monitor new bhavcopy file in designated directory and import csv file into Elasticsearch.
We used kibana based dashboards to visualize the stock activities.

## Required Downloads
1. Download and Install OpenJDK from https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u252-b09.1/OpenJDK8U-jdk_x64_windows_hotspot_8u252b09.msi
2. Download ES https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.7.0-windows-x86_64.zip
3. Download Kibana from https://artifacts.elastic.co/downloads/kibana/kibana-7.7.0-windows-x86_64.zip
4. Download logstash from https://artifacts.elastic.co/downloads/logstash/logstash-7.7.0.zip

## How to use
1.Start Elastic Search using <ES_DOWNLOAD_DIRECTORY>/bin/elasticsearch.bat
3.Download head plugin in chrome browser
2.Once ElasticSearch is connected, go to "Any Request Tab" and perform following step
Expand Query box and put: URL: "http://localhost:9200/bhavcopy" in first box and select "PUT" from the right side combo box.
Copy the contents from file CONFIG_TO_BE_IMPORTED/ES_Index_Settings.json in the big Text box above "Request" button, .
```
4. Click Request, button.

5.Copy CONFIG_TO_BE_IMPORTED/logstash_bhavcopy.conf in <LOGSTASH_EXTRACT_DIR>/config directory.

6. Edit file <LOGSTASH_EXTRACT_DIR>/config/logstash_bhavcopy.conf and change directory location "E:/FNOAnalysisUsingELK/bhavcopy/" and specify here full path of directory where you will put bhavcopy csv files downloaded from NSE site.

7.Start all services using StartServices.bat

8.Open Kibana portal using localhost:5601

9.Import the Kibana dashboards  from CONFIG_TO_BE_IMPORTED/KibanaDashboards.ndjson

10.Start using the dashboards. Currently two dashboards are available "FNO Open Interest Dashboard" gives tabular view of selected scrips whereas "Active FNO Scrips" provides graphical view of active Symbols based on changes in open interest.
