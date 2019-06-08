##엘라스틱 서치

https://geniedev.tistory.com/6

https://www.sysnet.pe.kr/2/0/11664

https://wedul.site/517

###실행

~~~
./bin/elasticsearch
~~~

* 열리는 포트

~~~
http://localhost:9200/
~~~

###중지

~~~
sudo lsof -i tcp:9200
~~~

###데이터 삭제

```
curl -XDELETE "http://localhost:9200/light_novel/"
```

###검색

```
http://localhost:9200/light_novel/_search?q=title:Novel&pretty
```

### 데이터 최초 생성

```
curl -XPUT "http://localhost:9200/light_novel/" -H "Content-Type: application/json" -d "{ \"light_novel\": { \"settings\" : { \"index\" : { } } } }"
```

###매핑 상태 확인

```
$ http://localhost:9200/light_novel/_mapping
{
    "light_novel": {
        "mappings": {
            "logs": {
                "properties": {
                    "@timestamp": {
                        "type": "date"
                    },
                    "@version": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        }
                    },
                    "adult": {
                        "type": "boolean"
                    },
                    "aladin_id": {
                        "type": "long"
                    },
                    "author_id": {
                        "type": "long"
                    },
                    "created_at": {
                        "type": "date"
                    },
                    "description": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        },
                        "analyzer": "openkoreantext-analyzer"
                    },
                    "hit_rank": {
                        "type": "long"
                    },
                    "id": {
                        "type": "long"
                    },
                    "isbn": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        }
                    },
                    "isbn13": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        }
                    },
                    "link": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        }
                    },
                    "publication_date": {
                        "type": "date"
                    },
                    "publisher_id": {
                        "type": "long"
                    },
                    "recommend_rank": {
                        "type": "long"
                    },
                    "sales_point": {
                        "type": "long"
                    },
                    "sales_price": {
                        "type": "long"
                    },
                    "standard_price": {
                        "type": "long"
                    },
                    "thumbnail": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        }
                    },
                    "title": {
                        "type": "text",
                        "fields": {
                            "keyword": {
                                "type": "keyword",
                                "ignore_above": 256
                            }
                        },
                        "analyzer": "openkoreantext-analyzer"
                    },
                    "updated_at": {
                        "type": "date"
                    }
                }
            }
        }
    }
}
```

###매핑 세팅

```
curl -XPUT "http://localhost:9200/light_novel/logs/_mapping" -H "Content-Type: application/json" -d "{\"logs\":{\"properties\":{\"@timestamp\":{\"type\":\"date\"},\"@version\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}}},\"adult\":{\"type\":\"boolean\"},\"aladin_id\":{\"type\":\"long\"},\"author_id\":{\"type\":\"long\"},\"created_at\":{\"type\":\"date\"},\"description\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}},\"analyzer\":\"openkoreantext-analyzer\"},\"hit_rank\":{\"type\":\"long\"},\"id\":{\"type\":\"long\"},\"isbn\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}}},\"isbn13\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}}},\"link\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}}},\"publication_date\":{\"type\":\"date\"},\"publisher_id\":{\"type\":\"long\"},\"recommend_rank\":{\"type\":\"long\"},\"sales_point\":{\"type\":\"long\"},\"sales_price\":{\"type\":\"long\"},\"standard_price\":{\"type\":\"long\"},\"thumbnail\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}}},\"title\":{\"type\":\"text\",\"fields\":{\"keyword\":{\"type\":\"keyword\",\"ignore_above\":256}},\"analyzer\":\"openkoreantext-analyzer\"},\"updated_at\":{\"type\":\"date\"}}}}"
```

##로그스테시 

###설치

https://www.elastic.co/kr/downloads/logstash 로 설치

mysql-connector-java-5.1.47 다운 후 lib 폴더에 jar 파일 이동

###보안 키스토어 실행

* 비밀번호 보호
* https://www.elastic.co/guide/en/logstash/current/keystore.html

~~~
bin/logstash-keystore create
~~~

~~~
./bin/logstash-keystore add ES_MYSQL_PASSWORD
./bin/logstash-keystore add ES_MYSQL_HOST
./bin/logstash-keystore add ES_MYSQL_USER

~~~

###실행

~~~
./bin/logstash -f logstash.conf
~~~

###종료

~~~
ps aux|grxp logstash
~~~