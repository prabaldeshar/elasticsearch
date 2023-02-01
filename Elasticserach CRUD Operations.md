### Creating index

Syntax:

```bash
PUT Name-of-the-Index
```

Example:
```
PUT books
```


Performing CRUD Operations with Elasticsearch and Kibana

Creating Pipeline to add Timestamp to the data (this is required for Kakfa)

```

PUT _ingest/pipeline/timestamp
{
	"description": "Creates a timestamp when a document is initially indexed",
	"processors": [{
	"set": {
	"field": "_source.timestamp",
	"value": "{{_ingest.timestamp}}"
		}
		}
]
}
```
Using timestamp
```
PUT test-index/_doc/45?pipeline=timestamp
{
   "my_field": "test numero 45",
   "my_field_2": "test"
}
```

### C- CREATE

Index a document (Adding a document to an index)

When indexing a document, both HTTP verbs POST or PUT can be used
1. Use POST when you want Elasticsearch to autogenerate an id for your document.
Syntax:
```
POST Index_Name/_doc
{
	"field": "value"
}
```

Example:
```
POST books/_doc
{
	"author": "Prabal",
	"title": "My Book"
}
```

2. Use PUT if you want  to assign a specific id to your document.

Syntax:
```
PUT Index-Name/_doc/id-you-want-to-assign
{
	"field": value
}
```

Example:
```
PUT books/_doc/1
{
	"author": "Name 1",
	"title": "Book 1"
}
```

You might want to specify the id to the document when you are indexing data with natural identifier. For example  you are indexing student data where each student has a unique id. It might be easier to idenitity each student with their roll no. Hence the id is uniform and not random.

Create more documents. Also create a document with the id that has been previously  used, using the PUT request and show that it simply updates the document with the existing id

"_version" : 3 sepcifies how many times your document has been created, updated or deleated.

#### create Endpoint
If you don't want your document to be overwritten then you can use the create endpoint.

```
PUT Index-Name/_create/id-of-doc
{
	"field": "value"
}
```

Example:
```
PUT books/_create/1
{
	"author": "John",
	"Book": "Programming"
}
```
### R - READ

Read a document

```
GET Index-Name/_doc/id-of-the-doc
```

Example:
```
GET books/_doc/1
```


"_version" : 3 sepcifies how many times your document has been created, updated or deleated.

### U - Update

Update the existing document:

Syntax

```
POST Index-Name/_update/id-of-the-doc-to-update
{
	"doc": {
		"field": "value"
	}
}
```

Example:
```
POST books/_update/1
{
	"doc": {
		"title": "Updated Title"
	}
}
```

### D-DELETE
Delete a document

Syntax:
```
DELETE Index-Name/_doc/id-of-doc-to-delete
```

Example:
```
DELETE books/_doc/1
```

