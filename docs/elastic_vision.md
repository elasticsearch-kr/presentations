# Elastic Vision

- This is not an official informations.
- check : https://www.elastic.co/blog for official release.

##What's new in elasticsearch 2.0

- Support for Thrift and Memcached protocols : **features before REST API**
- murmur3 : calculate hashcode. - check elastic.co
- size : ability of calculate size of `_source`

###conflicting field mapping

- 1.X : title, name.title - will pick one of them.
- 2.0 : different types == different property.
- 2.0 cannot use typename.*
  - type_one.name / type_two.name => name : must have the same attributes.

###Query and Filter
- no more : filteres
- 2.0 : bool
  - must : relevancy(신뢰도)
  - filtered : no relevancy

###Two phase execution
- Approximation phase
- Verification phase
```
{
  "bool" : {
    "must": [{
      "match_phrase": {
        "body": "quick fox"  ❶
      }, {
      "match_phrase": {
        "body": "brown dog"  ❷
      }
    }]
  }
}
```

- all docs count : 100
  - 1.X
    1. quick, fox - all docs    100 > 30
    2. find docs "quick" in front of "fox" - expensive 30
    3. find all docs "brown" and "dog"    100 > 20
    4. filter docs "brown" in front of "dog" - expensive  20
  - 2.0
    1. find all docs "quick", "fox", "brown", "dog"   100 > 10
    2. filter docs "quick" front "fox", "brown" front "dog" - expensive  10

###Reliability
- fsync : os operation. - data actually written to disk.
- transaction log :
  - lucene index - transaction log.
  - write index => transaction log
- fsync in index will take time. - every half our or so.
- commit is expensive.
- transaction log - less expensive.

- circle box : path.data:""

###Units are required all settings
- "refresh_interval" : "5" - 5 millisecond.
- 2.0 -> "refresh_interval" : "5s"

###Pipeline aggregation
- Derivatives(파생)
- holt winters (겨울숲)
- prediction (예측)
- put operation between aggs.
  - without pipline : separate bus long distanse & short dist.
  - with pipeline : can calculate bus speed, max number, minimun people in time amount.. so on.

###Plugins
- all plugins maintain with core ES. version no, URL ...

## UI - Kibana/Marvel
### Kibana 4.2

1. Field Formatter
2. Visualization
3. Apps

Kibana is going to control all applications.
Shield, Alert - everything will be controled with Kibana.
Kibana beccomes more like product.

##Logstash
### Logstash & Beats

- Logstash
  - input
  - filter
  - output: Elasticsearch

- Beats
  - input: Packet/File/Top Beat
  - output: Logstash

- Logstash
  - filter
  - output: Elasticsearch

###ELK Stack in one product.

Elastack ?
