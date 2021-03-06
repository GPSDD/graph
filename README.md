# Resource Watch Knowledge Graph

![screen shot 2017-06-07 at 3 06 04 pm](https://user-images.githubusercontent.com/545342/26879898-fb6500e6-4b92-11e7-8bdc-8dc469b1cf2b.png)

## Online Data browser

http://104.131.87.76:7474/browser/

## Data model

### Current node types

* CONCEPT
* DATASET

### Current relationship types

* RELATED_TO _This is a generic relationship used whenever it's not possible to use one of the more specific types_
* TYPE_OF
* PART_OF
* LOCATED_AT
* TAGGED_WITH
* AFFECTS
* IS_A
* CAN_AFFECT
* IS_INVOLVED_IN
* IS_SIMILAR_TO

### Current database schema

![screen shot 2017-06-07 at 4 00 13 pm](https://user-images.githubusercontent.com/545342/26882355-76464606-4b9a-11e7-85eb-ec5326379df1.png)

## Database creation

The knowledge graph of concepts is generated from the information stated in [this Google Spreadsheet](https://docs.google.com/a/vizzuality.com/spreadsheets/d/1awsO5aPEOv_OEFTakIhn-Ej7RFw46UP-jUWXnskPRqk/edit?usp=sharing).
 
### Step by step guide to generate the necessary files

1. Download the spreadsheet above as a .tsv file
2. Run `com/vizzuality/ExportToCSVFiles.java` with the path to the TSV file as its sole argument
3. The application will generate several `.csv` files inside the `csv_files` folder


### Step by step guide to import the Graph

1. Download the aforementioned spreadsheet as a TSV file
2. Upload it to the server using scp
3. Copy it to the folder called **import** that is located inside the Neo4j installation
4. Download the JSON file resulting from this request to the WRI API https://api.resourcewatch.org/dataset/?app=rw&includes=vocabulary
5. Execute the JAVA program: [src/main/java/com/vizzuality/CreateDatasetTagCSV.java]
6. Upload the resulting file to the server using scp
7. Copy it to the folder called **import** that is located inside the Neo4j installation
8. Execute the various Cypher statements included in the file: [ImportDBCypher.txt](ImportDBCypher.txt)

> Steps 4-7 can be skipped in the case you're not interested in adding the screenshot of datasets and their tags as a proof of concept


## Examples of queries

### Get all concepts that are descendants of the term 'energy'

```
MATCH (n:CONCEPT)-[*]->(e:CONCEPT {id: 'energy'})
RETURN n,e
```

![screen shot 2017-06-07 at 4 04 00 pm](https://user-images.githubusercontent.com/545342/26882540-f684425a-4b9a-11e7-83b9-46b204188045.png)

### Get all datasets tagged with water or any of the descendants of water

```
MATCH (d:DATASET)-[:TAGGED_WITH]->(c:CONCEPT)-[*]->(c2:CONCEPT)
WHERE c.id='water' OR c2.id='water'
RETURN d,c,c2
```


![screen shot 2017-06-07 at 4 29 42 pm](https://user-images.githubusercontent.com/545342/26883827-8dfe50a0-4b9e-11e7-9338-fbe176e4f0c1.png)

### Whole graph

```
MATCH p=()-[]-() return p
```
> Not recommended for computers with performance problems


