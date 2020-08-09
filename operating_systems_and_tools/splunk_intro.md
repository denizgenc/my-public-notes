# Splunk 4 Rookies
Delivered at Softcat, 2019-09-11 

Objective - take log data/machine data and add value to it through visualisations etc

Covering basic topics. If you want something more in-depth, talk to Softcat or go to "Splunk for
Ninjas" session hosted by splunk at their Paddington office

Traditional Data Types
- Relational data (SQL)
- Reference data
  - These two represent about 20% of all data

Splunk doesn't use these data types - uses raw files, and allows you to query it; Splunk applies a
schema on the fly to enable the query to work, and then the raw log remains at the end. No backend
database

Splunk Forwarders
- Lightweight agents send data to Splunk Indexers

Splunk Indexers
- Recieve and store data
- Can be clustered to replicate  data
- Auto load balanced

Splunk Search Heads
- This can be seperate from the indexer or could be on the same node. The more data you have, the
  better it is to separate them
- Does the processing for a search

What's a Universal Forwarder?
- Reliable collection of data from remote locations
- Includes methods for collecting from different sources (Files, ports, etc)
- It might be that the Universal Forwarder isn't good enough for your needs, but that's rare

In the search and reporting app:  
[Top right of page] Settings -> Add Data 

Methods of adding the data at the bottom:
- Upload (a static file with fixed log points)
- Monitor (the files, directories, and ports on the instance that is running Splunk)
- Forwarder

"Source Type" is Splunk's way of understanding the structure of the log data
- It can either be "Automatic" (Splunk selects one of the source types that are already defined,
  which can either be pre-installed with Splunk or defined by you), or selected manually by you.

An "Index" is a volume inside the indexer.
- Splunk recommends that you should 'Consider using a "sandbox" index as a destination if you
  have problems determining a source type for your data.'

Splunk determines interesting fields in the searches by determining which fields are seen in at
least 20% of events

Splunk's Search Processing Language Structure:  
`[command] [function] search terms [ | ]`
  - Pipes work like Unix pipes
  - After the first pipe, doing any extra searches (to further "filter" the results) requires an
    explicit `search` command (the first part of every search has an implict `search` at the
    start)

Best Practices: Optimise your Searches
- Limit to specific time window
- Restrict your search to a specific host, index, source, sourcetype, or splunk\server
- Limit the quantity of data retireved by your search, by piping into `head 1000`
- Inclusion is better than exclusion -> avoid using NOTs

Predicting the future with splunk:  
`action=purchase | timechart count span=5m | predict count future_timespan=24`
  - `timechart [field] span=5m` divides the time range into 5 minute "blocks"
  - `predict [field] future_timespan=24` predicts 24 "blocks" into the future

Settings -> Lookups -> Lookup table files
  - These files are like external CSV files
  - They can be restricted per-app, so if you're trying to find something specific, select "All"
    in the App drop down

Enriching data with the lookup command:  
`lookup` lookup\file.csv field\name
  - This will add the fields from the lookup file into your search.
  - If you don't know what those fields are... tough luck! See if you can find the lookup file
    to see what the field names are

Obtaining location info with the `iplocation` and `geostats` commands:  
`<your search> | iplocation ip\field | geostats count by [City|Region|Country]`
  - `iplocation` adds muliple fields - City, Region, Country, longitude, latitude (more?) to the
    results
  - `geostats` uses the longitude and latitude information to plot data on a map

Resources:
- splunkbase.splunk.com - apps for Splunk
- answers.splunk.com - StackOverflow, but for Splunk
- dev.splunk.com - allows you to publish your own apps
- docs.splunk.com - documentation for Splunk (reference, tutorials, use cases, references...)
- splunk.com/education - training and certification

