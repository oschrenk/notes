# Solr #

## Installation ##

On OS X

	brew install solr

On Ubuntu

	wget http://apache.mirrors.hostinginnederland.nl/lucene/solr/4.3.1/solr-4.3.1.tgz
	tar -zxvf solr-4.3.1.tgz
	cd solr-4.3.1

## Starting Solr ##

	cd example
	java -jar start.jar

Open a browser and point it to [localhost:8983/solr](http://localhost:8983/solr/#/)

## Indexing ##

All data you want Solr to index needs to be converted to an XML document SOlr can understand. A very simple example

	<doc>
	  <field name="id">9885A004</field>
	  <field name="name">Canon PowerShot SD500</field>
	  <field name="manu">Canon Inc.</field>
	  ...
	  <field name="inStock">true</field>
	</doc>

Every document can then be send via Http POST to Solr for parsing.

## Querying ##

You can query Solr with simple Http GET requests

	http://localhost:8983/solr/select?q=*:*&wt=json

- `*:*` Queries all documents
- `wt=json` Formats response as JSOn

Using the example `monitor.xml` document from the distribution you get something like

	{
    "responseHeader": {
        "status": 0,
        "QTime": 0,
        "params": {
            "wt": "json",
            "q": "*:*"
        }
    },
    "response": {
        "numFound": 1,
        "start": 0,
        "docs": [
            {
                "id": "3007WFP",
                "name": "Dell Widescreen UltraSharp 3007WFP",
                "manu": "Dell, Inc.",
                "includes": "USB cable",
                "weight": 401.6,
                "price": 2199,
                "popularity": 6,
                "inStock": true,
                "store": "43.17614,-90.57341",
                "cat": [
                    "electronics",
                    "monitor"
                ],
                "features": [
                    "30\" TFT active matrix LCD, 2560 x 1600, .25mm dot pitch, 700:1 contrast"
                ]
            }
        ]
    }
	}

back.

To retrieve only the `name` and `id` of all documents with `inStock = false` call `http://localhost:8983/solr/select?q=inStock:false&wt=json&fl=id,name`

	{
    "responseHeader": {
        "status": 0,
        "QTime": 2,
        "params": {
            "wt": "json",
            "q": "inStock:false",
            "fl": "id,name"
        }
    },
    "response": {
        "numFound": 4,
        "start": 0,
        "docs": [
            {
                "id": "F8V7067-APL-KIT",
                "name": "Belkin Mobile Power Cord for iPod w/ Dock"
            },
            {
                "id": "IW-02",
                "name": "iPod & iPod Mini USB 2.0 Cable"
            },
            {
                "id": "EN7800GTX/2DHTV/256M",
                "name": "ASUS Extreme N7800GTX/2DHTV (256 MB)"
            },
            {
                "id": "100-435805",
                "name": "ATI Radeon X1900 XTX 512 MB PCIE Video Card"
            }
        ]
    }
	}

## Caching ##

Solr caches are associated with an Index Searcher. Caching configuration is set-up in the Query section of `solrconfig.xml`

There are currently three cache implementations

— `solr.LRUCache` (LRU = Least Recently Used in memory)
- `solr.FastLRUCache`
- `solr.LFUCache` (Least Frequenty Used)


### FilterCache ###

- stores unordered sets of document IDs that match the key (usually queries)
- stores the results of any filter queries (`fq` parameters) that Solr is  asked to execute. Each filter is executed and cached separately. When it's time to use them to limit the number of results returned by a query, this is done using set intersections.
- may be used for sorting if the `<useFilterForSortedQuery/>` config option is set to true in `solfconfig.xml`
- is generally used in other places whenever the complete set of documents that match a query is required, such as `facet.query` and `group.query` commands
- Adding the localParam flag of `{!cache=false}` to a query will prevent the filterCache from being consulted for that query

### FieldValueCache ###

- low level cache
- primarily used by faceting
- keys of the cache are field names, and the values are large data structures mapping docIds to values

### QueryResultCache ###

- stores ordered sets of document IDs — the top N results of a query ordered by some criteria.

### DocumentCache ###

- stores Lucene Document objects that have been fetched from disk.
- should always be greater than `<max_results> * <max_concurrent_queries>`, to ensure that Solr does not need to refetch a document during a request.
- the more fields you store in your documents, the higher the memory usage of this cache will be

### Deep Caches ###

#### FieldCache ####

- used by Lucene
- not conifurable


http://engineering.bloomreach.com/solr-compute-cloud-an-elastic-solr-infrastructure/
