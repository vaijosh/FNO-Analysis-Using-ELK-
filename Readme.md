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

2.Create ES index bhavcopy with following configs

```
{
     "settings" : {
         "number_of_shards" : 5,
 		"refresh_interval" : "5s"
     },
     "mappings" : {
         "properties" : {
             "INSTRUMENT" : { "type" : "keyword" },
 			"SYMBOL" : { "type" : "keyword" },
 			"EXPIRY_DT" : { "type" : "date" },
 			"STRIKE_PR" : { "type" : "float" },
 			"OPTION_TYP" : { "type" : "keyword" },
 			"OPEN_INT" : { "type" : "long" },
 			"CHG_IN_OI" : { "type" : "float" },
 			"TIMESTAMP" : { "type" : "date" }	
         }
     }
 }
```

3.create logstash configs logstash_bhavcopy.conf

```
# Sample logstash config file to parse bhavcopy csv file. It will parse FNO bhavcopy csv files are downloaded in E:/FNO Analysis Using ELK/bhavcopy directory 
input {
	file {
		path => "E:/FNOAnalysisUsingELK/bhavcopy/*.csv"
		start_position => beginning
	}
}
filter {
    csv {
        columns => [
                "INSTRUMENT",
				"SYMBOL",
				"EXPIRY_DT",
				"STRIKE_PR",
				"OPTION_TYP",
				"OPEN",
				"HIGH",
				"LOW",
				"CLOSE",
				"SETTLE_PR",
				"CONTRACTS",
				"VAL_INLAKH",
				"OPEN_INT",
				"CHG_IN_OI",
				"TIMESTAMP"
        ]
		separator => ","
    }
	prune {
		whitelist_names  => [ 
			"INSTRUMENT",
			"SYMBOL",
			"EXPIRY_DT",
			"STRIKE_PR",
			"OPTION_TYP",
			"OPEN_INT",
			"CHG_IN_OI",
			"TIMESTAMP"
		]
	}
	prune {
		blacklist_values => [ "INSTRUMENT", "INSTRUMENT",
						  "SYMBOL", "SYMBOL",
						  "EXPIRY_DT", "EXPIRY_DT",
						  "STRIKE_PR","STRIKE_PR",
						  "OPTION_TYP", "OPTION_TYP",
						  "OPEN", "OPEN",
						  "HIGH", "HIGH",
						  "LOW", "LOW",
						  "CLOSE", "CLOSE",
						  "SETTLE_PR", "SETTLE_PR",
						  "CONTRACTS", "CONTRACTS",
						  "VAL_INLAKH", "VAL_INLAKH",
						  "OPEN_INT", "OPEN_INT",
						  "CHG_IN_OI", "CHG_IN_OI",
						  "TIMESTAMP", "TIMESTAMP"
						  ]
	}
	mutate {
		uppercase => ["EXPIRY_DT"]
	}
	date {
		match=> ["EXPIRY_DT","dd-MMM-yyyy"]
		target=> "EXPIRY_DT"
	}
	date {
		match=> ["TIMESTAMP","dd-MMM-yyyy"]
		target=> "TIMESTAMP"
	}
}
output {
  elasticsearch {
	hosts => ["http://localhost:9200"]
    index => "bhavcopy"
  }
}
```

4.Copy logstash_bhavcopy.conf in conf directory of logstash.

5.Start all services using StartServices.bat

6.Open Kibana portal using localhost:5601

7.Import the Kibana dashboards  from CONFIG_TO_BE_IMPORTED/KibanaDashboards.ndjson

8.Start using the dashboards. Currently two dashboards are available "FNO Open Interest Dashboard" 
gives tabular view of selected scrips whereas "Active FNO Scrips" provides graphical view of active
 Symbols based on changes in open interest.    
     
