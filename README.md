HBase REST API Scanner JSON Example
====================

##Introduction

When query from HBase, Scanner is the most commonly used tools. But few parts of the StarGate official doc mentioned scanner, not to mention the JSON version of scanner. I guess the reason for this is that scanner API is stateful. When the REST gate way is a group of servers, it is hard to maintain the state between gateway servers. In fact, in this situation, the REST only support the stateless part of the API. So just use Thrift API.

HBase REST API support multiple Content-Type, such as XML, JSON and Protobufs, which is asigned by the proper HTTP header. Attention, unlike XML and Protobufs version, In JSON version ,the value must be encode in base64.

Above all, if you still want to use the REST JSON version scanner. I will provide some examples fro the JSON version scanner.

##Sample

###Prefix match scanner
REST provide a simple prefix match scanner, this can retrieve all rows begin with the prefix

```
GET  http://url/table/row-prefix*[/column]

curl -v -H"Content-Type:application/json" http://localhost:8080/table/row-key-prefix*/col:info

{"Row":[{"cell":[{"$":" ","column":"","timestamp":" "},...],"key":" "},...]}
```

###Create basic scannner
POST or PUT with the ../table/scanner with param in json version will create a scanner

```
POST  http://url/table/scanner 

curl -v -XPOST -H"Content-Type:application/json" http://localhost:8080/table/scanner -d '{"startRow":"[Base64 Content]","endRow":"[Base64 Content]"}'

...
<HTTP/1.1 201 Created
<Location: http://localhost:8080/table/scanner/<scanner-id>
...
```
### Scanner query
use the response Location to scan by page
```
GET  http://url/table/scanner/<scanner-id> 

curl -v -XPOST -H"Content-Type:application/json" http://localhost:8080/table/scanner -d '{"startRow":"[Base64 Content]","endRow":"[Base64 Content]"}'

{"Row":[{"cell":[{"$":" ","column":"","timestamp":" "},...],"key":" "},...]}
```

##Reference
*[HBase Stargate wiki](http://wiki.apache.org/hadoop/Hbase/Stargate)
*[How-to: Use the Apache HBase REST Interface](http://blog.cloudera.com/blog/2013/03/how-to-use-the-apache-hbase-rest-interface-part-1/)

