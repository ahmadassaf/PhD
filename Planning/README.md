A Framework for Generating Enhanced Dataset Descriptions 

## Basic Plan
- Set up evaluation protocol. This will serve as the basis to validate the assumptions we are making along the way
- Evaluate **SEED** against Freebase and DBpedia lookup for usage in annotating datasets
- Crawl a datahub given its URI with options to parse the whole hub or a specific group in it or a specific dataset
	- The descriptions of the datahub metadata are persisted in a database 
	- We limit ourselves to datasets with CSV downloadable data
	- We limit also the set of CSV to those who are validated against a CSV lint to ensure we work with reasonably clean set
	- Perform sampling on the data if above a certain threshold, Sampling can be naive in the beginning but can be refined in the future
- Perform **SEED** on the sampled data and create topic models on top of that
- Perform basic data profiling techniques as well (min, max, mean ... etc.)
- Expose the CSV metadata (investigate the [CSV working group](http://www.w3.org/2013/05/lcsv-charter.html) recommendation)

## Detailed Plan ##

The open data movement was followed by a massive increase of datasets published via data hubs or portals. Up to this moment, it is up to the publisher to provide the dataset description. This description is then uploaded to the portal so that users will be able to search and get an overview of the dataset.

Linked Open Data added to this openness by allowing data to be interlinked and become more useful. The data published as of huge benefit to the enterprise "_**There is money in Linked Data**_"

There is no automatic way to generate parts of this description to left up some of the work from the publisher. The proposal is to provide a tool that will be able to take a dataset (CSV format, RDF or point to SPARQL endpoint) and it will examine the data on the instance level and will be able to provide suggested keywords or "tags" and high level themes. The tool will work on existing data portals, the input will be a datahub URI (currently we support CKAN based datahubs) afterwards the user will be able to choose:

- Parse all the dataset descriptions in that datahub
- Provide a group ID
- Provide a specific dataset ID or name

The tool performs a certain checks on the current metadata to see if there has been already a description, tags, license information, categories ... etc.

Afterwards will we run NER/NED on the extracted textual terms as well as keywords extraction.

In the case of CSV files we will use the knowledge to annotate the CSV with entities, types and relationships. This annotation will serve as an enrichment module that will help ad more background knowledge from reference knowledge bases like DBpedia. **[1]**

###Annotation Evaluation###

- Compare the results against a manually created golden standard
- Compare extracted keywords and categories against current description in the data hub
- Compare results against a search application to motivate the role of annotations for improved search results **[1]**

In the case of RDF data we need to check what kind of extra metadata we can add:

- List of vocabularies used in that dataset
- License information against the combined dataset and those linked to it **[5]**

###Dataset Profiling###

In addition to the metadata generated above, we want to add additional statistics about the data itself. These statistics differs from RDF to CSV data  **[12][14]** and can include SPARQL endpoint status **[13]**

###Dataset Categorization###

The extracted resources and keywords can be used so that we can define topics coverage as well as categories/themes for those dataset. in **[3]** **[4]** similar work has been done but with shortcomings. For example, sampling done is naiive and proper graph sampling methods should have taken into account **[7][8][9]** Moreover, for disambiguating (NED) textual terms he used [DBpedia Sptotlight](https://github.com/dbpedia-spotlight/dbpedia-spotlight/wiki) that doesnt capture context into account. Tools like [10] should have been taken into account. Lastly, he uses several topic modeling techniques that we want to compare against **[11]**

However, doing all this will allow us to add topic coverage metadata information for datasets but doesn't really help in grouping the datasets into a predefined set of themes. The idea is to "reverse engineer" the categorization on data portals like [data.gov](http://data.gov/) where they have a set of 18 defined themes and map datasets according to these.

###Categorization Evaluation###

- Compare the annotation process for RDF data on the datahub.io against the golden standard constructed in **[3][4]**
- Compare against a survey **[4]**
- Compare the results against a manually created golden standard

So far we have talked about profiling a dataset based on examining the data on the instance level. However, for Linked Open Data we have a new source of knowledge that can be further examined which is the ontologies used. 

@ghizlain already worked on trying to identify certain types of datasets (i.e. geography, event) by querying against predefined SPARQL templates. Further investigations could be:

- Automatic SPARQL templates generation from ontologies in LOV.
- Using metadata attached to ontologies in LOV to profile dataset by extracting the ontologies used in that dataset, for each RDF clause check against LOV metadata. 
- Using the data on the instance level to annotate vocabularies and compare that against metadata in LOV

###SEED (Semantic Enrichment of Enterprise Data)###

SEED is built upon DBpedia hosted on HANA with a contextual search service on top of that. However, the added value of SEED should be the integration of enterprise knowledge mined from HANA Live.

HANA Live is a manually created database of SAP Views across all of their systems with manually entered descriptions and tags. 

Currently the work we are doing is modeling the knowledge of HANA Live and create a mapping with those of external knowledge based like DBpedia and host them in SEED. Then we want to check if the semantic annotation process is better when using this enterprise reference information or not.

###Open Questions###

- Can we prove that enriching datasets, better categorization through topic model will improve entity linking? **[2]**

## Architecture Diagram ##

![Architecture Diagram](https://www.dropbox.com/s/gz3jmea5yq6nvft/architecture_diagram.png?dl=1)


## Thoughts to keep in mind
- The work Florian is doing in modeling the manually curated SAP business data and combining that with the general knowledge exposed in DBpedia
- Conference deadlines and that we have to aim for [ESWC](http://2015.eswc-conferences.org/), [WWW](http://www.www2015.it/) that are within the thesis time frame (Early November and late December). The WWW paper can describe the PhD workflow with SNARC, Wikipedia extractions ... etc. 
- The data quality paper and [ACM Journal](http://jdiq.acm.org/) and that it can be put on hold 
- Wikipedia tables using the extraction framework as well and to keep it in mind

## Deadlines
- **World Wide Web Conference WWW15** _Florence, Italy_  
	- ~~Abstract submission:November 3rd 2014~~
	- ~~Full Paper submission:November 10th 2014~~
    - [**Linked Data on the Web (LDOW2015)**](http://events.linkeddata.org/ldow2015/) March 15th 2015
- **European Semantic Web Conference ESWC15** January 17th 2015
- [**IJSWIS Special Issue on Dataset Profiling and Federated Search for Linked Data**](http://www.ijswis.org/?q=node/51) February 10th 2015
- [~~2014 International Workshop on Semantic Web for the Law~~](http://cs.unibo.it/sw4law2014)
	- October 30, 2014: Paper submission deadline (Hawaii time)
- [**Journal of Web Semantics: Sepcial Issue on Knowledge Graphs**](http://www.websemanticsjournal.org/index.php/ps/announcement/view/19) February 28th 2015
- [**2nd Workshop on Linked Data Quality (LDQ2015)**](http://ldq.semanticmultimedia.org/) March 6th 2015
- [**2nd International Workshop on Dataset PROFIling & fEderated Search for Linked Data**](http://www.keystone-cost.eu/profiles2015/) March 6th 2015

## Related Tools

- [Linked Dataset Profiling Tool](https://github.com/bfetahu/ldp_tool)
- [TRank](https://github.com/MEM0R1ES/TRank)
- [DBpedia Spotlight](https://github.com/dbpedia-spotlight/dbpedia-spotlight)
- [SampLD](http://data2semantics.github.io/GraphSampling/)
- [Graph Sampling](https://github.com/Data2Semantics/GraphSampling)
- [PROV-O-Matic](https://github.com/Data2Semantics/prov-o-matic)
- [Aether VoID Statistics Tool](http://demo.seco.tkk.fi/aether)
- [The Dynamic Linked Data Observatory](http://swse.deri.org/dyldo/)
- [Kanopy](http://uimr.deri.ie/sites/kanopy/)
- [voX, the Dataset Explorer](http://lab.linkeddata.deri.ie/vox/)
- [LODStats](https://github.com/ahmadassaf/LODStats#)
- [SPARQL ENDPOINTS STATUS](http://sparqles.okfn.org/)
- [TabLinker](https://github.com/Data2Semantics/TabLinker)
- [Apache Any CSV Extractor](https://any23.apache.org/dev-csv-extractor.html)

## References

[1] [Annotating and searching web tables using entities, types and relationships](http://www.vldb.org/pvldb/vldb2010/papers/R118.pdf)

[2] [Identifying Candidate Datasets for Data Interlinking](http://link.springer.com/chapter/10.1007/978-3-642-39200-9_29#)

[3] [A Scalable Approach for Efficiently Generating Structured Dataset Topic Profiles](http://www.l3s.de/~fetahu/publications/fetahu_eswc2014.pdf)

[4] [Automatic Domain Identification for Linked Open Data](http://knoesis.wright.edu/pascal/pub/domainIdentLOD13.pdf)

[5] [A Deontic Logic Semantics for Licenses Composition in the Web of Data](http://www-sop.inria.fr/members/Serena.Villata/Resources/icail2013.pdf)

[6] Sampling Very Large Knowledge Bases Using Network Properties

[7] [Sampling Techniques for Large, Dynamic Graphs](http://www.utdallas.edu/~emrah.cem/references/SamplingTechniquesForLargeDynamicGraphs.pdf)

[8] [Sampling A Large Network - How Small Can My Sample Be](http://snap.stanford.edu/class/cs224w-2012/projects/cs224w-036-final.pdf)

[9] [Sampling from Large Graphs](http://www.stat.cmu.edu/~fienberg/Stat36-835/Leskovec-sampling-kdd06.pdf)

[10] [TRank: Ranking Entity Types ](http://exascale.info/sites/default/files/entityTypes.pdf)[Using the Web of Data](http://exascale.info/sites/default/files/entityTypes.pdf)

[11] [Kanopy: Analysing the Semantic Network around Document Topics](http://sw.deri.ie/sites/default/files/publications/kanopy-demo.pdf)

[12] [LODStats – An Extensible Framework for High-performance Dataset Analytics](http://svn.aksw.org/papers/2011/RDFStats/public.pdf)

[13] [SPARQL Web-Querying Infrastructure: ](http://vmwebsrv01.deri.ie/sites/default/files/publications/paperiswc.pdf)[Ready for Action?](http://vmwebsrv01.deri.ie/sites/default/files/publications/paperiswc.pdf)

[14] [Generating and Viewing Extended VoID Statistical Descriptions of RDF Datasets](http://www.seco.tkk.fi/publications/2014/makela-aether-2014.pdf)
