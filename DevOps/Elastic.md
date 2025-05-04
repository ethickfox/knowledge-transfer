- Fully distributed data store
- Nodes (computers) working in concert
- Document data store with JSON
- Apache Lucene as a storage format

**Apache Lucene**

- Specialized database engine
- Search structured data and text blocks
- Document database - not as standardized as SQL

![Untitled 58.png](../Databases/NoSQL/_img/Untitled%2058.png)

How Cluster of Nodes Work?

- Leader node - in charge of shards
- Data node - hold the data itself
- Leaders are distributed for redundancy
- Data nodes are assigned as needed

![Untitled 1 16.png](Untitled%201%2016.png)

## DSL

It uses CRUD via REST

### GET

Returns all document information

```JavaScript
GET bowled_over/_doc/2
//result
{
  "_index": "bowled_over",
  "_id": "2",
  "_version": 2,
  "_seq_no": 4,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "first_name": "Edward2",
    "last_name": "Testovich",
    "average": 110
  }
}
```

Returns only document data

```JavaScript
GET bowled_over/_source/1
//result
{
  "first_name": "Edward",
  "last_name": "Testovich",
  "average": 190
}
```

**Search**

Searches in documents

```JavaScript
//filter_path=hits.hits._source to show only specified fields
GET bowled_over/_search?q=edward*&filter_path=hits.hits._source
//result
{
  "hits": {
    "hits": [
      {
        "_source": {
          "first_name": "Edward2",
          "last_name": "Testovich",
          "average": 110
        }
      },
      {
        "_source": {
          "first_name": "Edward",
          "last_name": "Testovich",
          "average": 190,
          "nickname": "lincoln"
        }
      },
      {
        "_source": {
          "first_name": "Test",
          "last_name": "Edwardovich",
          "average": 120
        }
      }
    ]
  }
}
```

```JavaScript
GET bowled_over/_search?q=average(>190 AND <220)&filter_path=hits.hits._source
//equal
GET /bowled_over/_search
{
	"query": {
		"query_string": (
			"query": "average (>190 AND <220)"
		}
	}
}
```

Match by specific field

```JavaScript
GET bowled_over_artist_guild/_search
{
	"query": {
		"match": {
			"passage": "bowl dinner cheer award"
		}
	}
}
```

Mapping

```JavaScript
GET bowled_over/_mapping
//result
{
  "bowled_over": {
    "mappings": {
      "properties": {
        "average": {
          "type": "float"
        },
        "first_name": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "last_name": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        },
        "nickname": {
          "type": "text",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        }
      }
    }
  }
}
```

### PUT

Updates existing document if id 2 exists, else creates new

```JavaScript
PUT bowled_over/_doc/2
{
  "first_name": "Edward2",
  "last_name": "Testovich",
  "average": 110.0
}

//result 
{
  "_index": "bowled_over",
  "_id": "1",
  "_version": 5,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 10,
  "_primary_term": 1
}
```

### POST

Creates new document or updates if id is set

```JavaScript
POST bowled_over/_doc/
{
  "first_name": "Test",
  "last_name": "Edwardovich",
  "average": 120.0
}
```

Search using SQL

```JavaScript
POST _sql?format=text
{
	"query": """
				SELECT Author, count(1) 
					FROM 'bowled_over' 
					WHERE Author like '%whiph%' 
					GROUP BY Author
	"""
}
POST _sql/translate //translates query to elastic dsl
```

### DELETE

Deletes index(collection) or element of index

```JavaScript
DELETE bowled_over
DELETE bowled_over/_doc/2
```

## Schema

  

![Untitled 2 12.png](../Software_Architecture/_img/Untitled%202%2012.png)

Create the schema

```Bash
PUT simple_schema 
{
	"mappings" : {
		"dynamic" : false,
		"properties" : {
			"age": {
				"type":"long"
			}
		}
	}
}
```

Nested

![Untitled 3 12.png](../Software_Architecture/_img/Untitled%203%2012.png)

![Untitled 4 9.png](../Software_Architecture/_img/Untitled%204%209.png)

![Untitled 5 9.png](../Software_Architecture/_img/Untitled%205%209.png)

Compute multiple fields for search\

Creates field monikers from first and last name

![Untitled 6 9.png](Untitled%206%209.png)

Fuzziness

![Untitled 7 7.png](Untitled%207%207.png)

![Untitled 8 7.png](Untitled%208%207.png)

![Untitled 9 7.png](Untitled%209%207.png)

Conditional pipelines

![Untitled 10 7.png](Untitled%2010%207.png)

![Untitled 11 7.png](Untitled%2011%207.png)

![Untitled 12 7.png](../Software_Architecture/_img/Untitled%2012%207.png)

  

![Untitled 13 7.png](../Software_Architecture/_img/Untitled%2013%207.png)

![Untitled 14 7.png](Untitled%2014%207.png)

search by several indexes

![Untitled 15 6.png](Untitled%2015%206.png)

Scripts

![Untitled 16 6.png](Untitled%2016%206.png)