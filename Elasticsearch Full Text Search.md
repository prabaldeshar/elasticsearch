### Search


**Retrive all document from  an index**
Syntax:
```
GET Index-Name/_search
```
Example:

```
GET news_headline/_search
```

### Add sample data to Elasticsearch

[Download link](https://www.kaggle.com/datasets/rmisra/news-category-dataset?resource=download)

Queries and Aggerations

Queries return the data of our search but aggregations returns the information about or data.

Example:

```
GET  news_headline/_search
{
	"aggregations": {
		"by_category": {
			"terms": {
				"field": "category",
				"size": 100
			}
		}
	}
}
```


### Full Text Queries

### Searching for search terms

**Match query** returns the document that match a provided text, number, date or boolean value. The `match`  query is a standard query for performing full text search . This query retreives documents that contain the search term in any way, shape or form. The order in which the search terms are found are not considered as priority.

Syntax:

```
GET Index-Name/_search
{
	"query": {
		"match": {
			"field-name": {
				"query": "Enter the search terms"
			}
		}
	}
}
```

Example:

```
GET news_headline/_search
{
	"query": {
		"match": {
			"short_description": {
				"query": "Burmese python"
			}
		}
	}
}
```

Searching for phrases using match query

Let's search for articles about Ed sheeran's song "Shape of you" using the match query.

Example:
```
GET news_headline/_search
{
  "query": {
    "match": {
      "headline": {
        "query": "Shape of you"
      }
    }
  }
}
```

To  get the exact match for the pharse we can use "match_pharse"
Example:
```
GET news_headline/_search
{
  "query": {
    "match_phrase": {
      "headline": {
        "query": "Shape of you"
      }
    }
  }
}
```

### Running a match query against multiple fields

The `multi_match` query builds on the `match` query to allow multi-field queries.

Syntax:
```
GET Index-Name/_search
{
  "query": {
    "multi_match" : {
      "query":    "Enter the search terms,
      "fields": [ 
	      "field-you-want-to-search", 
	      "field-you-want-to-search" 
      ]
    }
  }
}
```

Example:
```
GET news_headline/_search
{
  "query": {
    "multi_match" : {
      "query":    "Michelle Obama",
      "fields": [ 
	      "headline", 
	      "short_description",
	      "authors"
      ]
    }
  }
}
```


### Per-field boosting`

Boosting is the process by which you can modify the relevance of a document.  The relevance of the field can be boosted so that the field is given more importance during search.

This can be done by using the caret (^) symbol.

Example:
```
GET news_headline/_search
{
  "query": {
    "multi_match" : {
      "query":   "Michelle Obama",
      "fields": [ 
	      "headline^2", 
	      "short_description",
	      "authors"
      ]
    }
  }
}
```

Matches on `headline` field will have twice the weight as those on the `short_description` and `authors` field.

### Using `multi_match` query to search for a phrase

Since the `multi_match` uses the `match` query to search on all the given fields it might not always reutrn the exact match and give irrelevant results.

For example:
```
GET news_headline/_search
{
  "query": {
    "multi_match" : {
      "query":    "party planning",
      "fields": [ 
	      "headline^2", 
	      "short_description"
      ]
    }
  }
}
```

**Improving precision with pharse match**

You can improve the precision of a `multi_match query` by adding the "type":"phrase" to the query.

Example:
```
GET news_headline/_search
{
  "query": {
    "multi_match" : {
      "query":    "party planning",
      "fields": [ 
	      "headline^2", 
	      "short_description"
      ],
      "type": "phrase"
    }
  }
}

```