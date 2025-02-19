# FOR TESTING PURPOSES ONLY, DO NOT USE.

# CKAN Harvester for OAI-PMH

Original built from Open Search Data and made changes according to NFDI4Chem harvesting & adoption.

[![Build Status](https://travis-ci.org/openresearchdata/ckanext-oaipmh.svg?branch=master)](https://travis-ci.org/openresearchdata/ckanext-oaipmh)

This harvester has following additional aspects and features from the original harvester. 

- oai_datacite metadata schema for DataCite 4.0 and above 
- RDKit Module to generate cheminformatics.
- Storing chemi-metadata to database tables (migrated tables) 

## Instructions

### Installation

Use `pip` to install this plugin. 

To install ckanext-oaipmh:

Activate your CKAN virtual environment, for example:

    . /usr/lib/ckan/default/bin/activate

Clone the source and install it on the virtualenv

    git clone https://github.com/bhavin2897/ckanext-oaipmh.git 
    
    cd ckanext-oaipmh 
    
    pip install -e . 
        pip install -r requirements.txt

Add 'oaipmh_harvester' to the ckan.plugins setting in your CKAN config file (by default the config file is located at /etc/ckan/default/ckan.ini).

Restart CKAN. For example if you've deployed CKAN with Apache on Ubuntu:

    sudo service apache2 reload

or if using deployed production server

    restart Supervisor and Nginx


Make sure the ckanext-harvest extension is installed as well.

**Important: You need to have a sysadmin user called "harvest" on your CKAN instance!**

### Setup the Harvester

- add `oaipmh_harvester` to `ckan.plugins` in `ckan.ini`, if you haven't done as above & do not forget to restart server as menitoned above. 
- with the web browser go to `<your ckan url>/harvest/new`
- as URL fill in the base URL of an OAI-PMH conforming repository, e.g. https://oai.datacite.org/oai/

- select **Source type** `Dublin Core Harvester` or `DataCite OAI Harvester`
- if your OAI-PMH needs credentials, add the following to the "Configuration" section: `{"username": "foo", "password": "bar" } `
- if you only want to harvest a specific set, add the following to the "Configuration" section: `{"set": "baz"} `
- if you want to harvest data in a specific metadata format, add the following to the "Configuration" section: `{"metadata_prefix": "oai_dc"}` (currently `oai_datacite`,`oai_dc', `oai_ddi` are supported)
- if your OAI-PMH source does not support HTTP POST and you want to enforce HTTP GET, add the following to the "Configuration" section: `{"force_http_get": true}`  (defaults to `false`)
- if you want harvest during a time duration, use 
        `{"from": "2020-09-20T00:00:01Z" & "until": "2021-01-01T00:00:01Z"}`
  Please follow OAI-PMH guides line for using timestamps http://www.openarchives.org/OAI/openarchivesprotocol.html#DatestampsRequests
- Save
- on the harvest admin click **Reharvest**

NOTE: if requirements if an error message. Ignore errors and install the rest. You can use below command to install each line seperately. 

    cat requirements.txt | xargs -n 1 pip install

### Run the Harvester

On the command line do this:

- activate the python environment
- `cd` to the ckan directory, e.g. `/usr/lib/ckan/default/src/ckan`
- start the consumers:

```
ckan -c /etc/ckan/default/ckan.ini harvester gather_consumer &
ckan -c /etc/ckan/default/ckan.ini harvester fetch_consumer &
```

- run the job:

    `ckan -c /etc/ckan/default/ckan.ini harvester run`

The harvester should now start and import the OAI-PMH metadata.

## Running on Prod server

On the command line do this:

- activate the python environment
- `cd` to the ckan directory, e.g. `/usr/lib/ckan/default/src/ckan`
- start the harvester
-
    `ckan -c /etc/ckan/default/ckan.ini harvester run`
    
## Running Harvester using cron-jobs
TODO: Add results
