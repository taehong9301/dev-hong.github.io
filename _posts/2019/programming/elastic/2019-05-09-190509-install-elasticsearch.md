---
title: "MacOS Elasticsearch 7.0.1 설치하기"
excerpt: "간단하게 엘라스틱서치에 대해 소개하고, MacOS에 Elasticsearch 7.0.1를 설치하는 방법에 대해 다룹니다."
search: true
categories:
  - Programming
  - Elastic
tags:
  - Programming
  - Elastic
  - Elasticsearch
  - MacOS
last_modified_at: 2019-05-09T21:47:00+09:00
toc: true
toc_sticky: true
comments: true
header:
  teaser: https://user-images.githubusercontent.com/26136312/57454888-da0d0480-72a4-11e9-85a4-6a4d482bb6a2.png
elasticsearch_gallery:
  - url: https://user-images.githubusercontent.com/26136312/57455997-6fa99380-72a7-11e9-8036-3ae36640c847.png
    image_path: https://user-images.githubusercontent.com/26136312/57455997-6fa99380-72a7-11e9-8036-3ae36640c847.png
    alt: "Elasticsearch1"
    title: "Elasticsearch1"
elasticsearch_gallery2:
  - url: https://user-images.githubusercontent.com/26136312/57456036-8819ae00-72a7-11e9-8bb9-d279c4076a00.png
    image_path: https://user-images.githubusercontent.com/26136312/57456036-8819ae00-72a7-11e9-8bb9-d279c4076a00.png
    alt: "Elasticsearch2-1"
    title: "Elasticsearch2-1"
  - url: https://user-images.githubusercontent.com/26136312/57456037-8819ae00-72a7-11e9-80df-5ee8e4918bb8.png
    image_path: https://user-images.githubusercontent.com/26136312/57456037-8819ae00-72a7-11e9-80df-5ee8e4918bb8.png
    alt: "Elasticsearch2-2"
    title: "Elasticsearch2-2"
install_gallery:
  - url: https://user-images.githubusercontent.com/26136312/57454888-da0d0480-72a4-11e9-85a4-6a4d482bb6a2.png
    image_path: https://user-images.githubusercontent.com/26136312/57454888-da0d0480-72a4-11e9-85a4-6a4d482bb6a2.png
    alt: "Install Elasticsearch"
    title: "Install Elasticsearch"
---

## Elasticsearch란?

`Elasticsearch`는 데이터를 쉽게 처리할 수 있도록 도와주는 서비스입니다. 그리고 `Elastic Stack`도 있는데, `Kibana` `Elasticsearch`, `Beats`, `Logstash`를 합쳐서 `Elastic Stack`이라 부릅니다. (이외의 다른 제품들을 `X-Pack`으로 묶어서 부름)

엘라스틱서치는 엘라스틱의 심장이라 할 수 있을만큼 중요한 역할을 합니다. 데이터 저장, 검색, 쿼리를 이용하여 데이터를 다룰수 있습니다. 실시간성, 데이터 무결성 등의 장점이 있고 이미 많은 기업들이 사용하고 있는 서비스 입니다.

엘라스틱서치의 검색엔진은 아파치 루씬을 이용했고, 아파치 루씬은 Java기반으로 만들어졌습니다. (그래서 엘라스틱서치를 설치하기 위해서는 `JDK`가 필요함)

<i class="fas fa-feather-alt"></i> **참고** OpenJDK 1.8 설치는 <a href="/programming/190421-install-openjdk" target="_blank">여기</a>를 참고하세요.
{: .notice--info}

엘라스틱서치에서는 데이터를 샤드(`Shard`)라 부릅니다. 그리고 `Elasticsearch`가 실행된 프로세스를 노드(`Node`)라고 부릅니다.
클러스터링을 하여 데이터를 저장할때 샤드를 각각의 노드에 저장합니다. 그리고 **복사본을 원본과 다른 위치에 있는 노드에 저장**합니다.

{% include gallery id="elasticsearch_gallery" caption="출처 https://www.elastic.co/" %}

만약 하나의 노드에 장애(네트워크 장애와 같은 경우)가 생겨 사용하지 못하는 경우, 장애가 생긴 노드에 위치한 샤드들을 다시 복사하여 각각의 살아있는 노드에 **재분배**합니다. 그렇기때문에 **데이터의 무결성을 보장**할 수 있습니다.

{% include gallery id="elasticsearch_gallery2" caption="출처 https://www.elastic.co/" %}

## Elasticsearch 설치

<a href="https://www.elastic.co/downloads/elasticsearch" target="_blank">https://www.elastic.co/downloads/elasticsearch</a>에서 `Elasticsearch`를 설치할 수 있습니다.

사이트를 들어가면 다운로드를 받을 수 있는 링크가 여러개 존재합니다.

{% include gallery id="install_gallery" caption="Install elasticsearch." %}

본 글에서는 맥OS에 설치를 하기 때문에 `MACOS`를 눌러 `tar` 파일을 설치합니다.

### JDK 버전 확인

압축을 해제하기 전에 jdk를 설치해야 합니다. `jdk1.8` 이상을 설치를 한뒤에 아래작업을 진행해주세요.

```bash
$ java -version
java version "1.8.0_201"
Java(TM) SE Runtime Environment (build 1.8.0_201-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.201-b09, mixed mode)
```

### 압축해제 및 실행

다운받은 `tar` 파일을 압축해제 합니다.

```bash
$ tar xfz elasticsearch-7.0.1-darwin-x86_64.tar.gz
```

압축해제한 파일을 들어가 파일을 확인해봅니다.

```bash
$ cd elasticsearch-7.0.1
$ ls
LICENSE.txt	README.textile	config		lib		modules
NOTICE.txt	bin		jdk		logs		plugins
```

- bin: 엘라스틱서치 실행파일이 들어있다.
- config: elasticsearch.yml 과 같이 환경설정 파일들이 들어있다.

이제 엘라스틱서치를 실행해봅시다.

```bash
$ cd bin
$ ./elasticsearch
OpenJDK 64-Bit Server VM warning: Option UseConcMarkSweepGC was deprecated in version 9.0 and will likely be removed in a future release.
[2019-05-09T22:26:23,732][INFO ][o.e.e.NodeEnvironment    ] [Taehongui-MacBook-Pro.local] using [1] data paths, mounts [[/ (/dev/disk1s1)]], net usable_space [413.6gb], net total_space [446.9gb], types [apfs]
...생략...
[2019-05-09T22:26:42,234][INFO ][o.e.x.i.a.TransportPutLifecycleAction] [Taehongui-MacBook-Pro.local] adding index lifecycle policy [watch-history-ilm-policy]
[2019-05-09T22:26:42,449][INFO ][o.e.l.LicenseService     ] [Taehongui-MacBook-Pro.local] license [1bf39fe2-0ee2-49f9-89d9-04e4b1f85735] mode [basic] - valid
```

### 동작 확인

자 그럼 다른 터미널을 띄어서 제대로 동작하는지 확인을 합니다.

```bash
$ curl -XGET localhost:9200
{
  "name" : "Taehongui-MacBook-Pro.local",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "oFrfsoSsSySoTT7Bj9vnEQ",
  "version" : {
    "number" : "7.0.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "e4efcb5",
    "build_date" : "2019-04-29T12:56:03.145736Z",
    "build_snapshot" : false,
    "lucene_version" : "8.0.0",
    "minimum_wire_compatibility_version" : "6.7.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```

위와 같은 결과가 나온다면 정상적으로 동작하는 것.

<i class="fas fa-feather-alt"></i> **Note** 따로 설정하지 않으면 기본적으로 포트번호는 `9200`으로 설정이 된다.
{: .notice--info}

## 클러스터 환경 만들기

이번에는 여러개의 노드를 만들어서 클러스터링을 해보도록 하겠습니다.

위에서 압축해제했던 파일을 node-1로 이름을 바꿉시다.

```bash
$ mv elasticsearch-7.0.1 node-1
$ ls
elasticsearch-7.0.1-darwin-x86_64.tar.gz	node-1
```

그리고, 2개의 노드를 더 만듭니다.

```bash
$ tar xfz elasticsearch-7.0.1-darwin-x86_64.tar.gz
$ mv elasticsearch-7.0.1 node-2
$ ls
elasticsearch-7.0.1-darwin-x86_64.tar.gz	node-1						node-2
```

```bash
$ tar xfz elasticsearch-7.0.1-darwin-x86_64.tar.gz
$ mv elasticsearch-7.0.1 node-3
$ ls
elasticsearch-7.0.1-darwin-x86_64.tar.gz	node-2
node-1						node-3
```

총 `node-1`, `node-2`, `node-3`의 노드 3개를 만들었습니다. 그리고 `node-1`의 설정파일을 수정하여 클러스터 이름과 노드 이름을 수정합니다.

```bash
$ cd node-1
$ cd config
$ vi elasticsearch.yml
```

```bash
...생략...
# ---------------------------------- Cluster -----------------------------------
#
# Use a descriptive name for your cluster:
#
cluster.name: es-1
#
# ------------------------------------ Node ------------------------------------
#
# Use a descriptive name for the node:
#
node.name: node-1
#
# Add custom attributes to the node:
#
#node.attr.rack: r1
#
...생략...
```

본 글에서는 클러스터의 이름은 `es-1`, 그리고 노드의 이름은 `node-1`로 설정했습니다. 마찬가지로 다른 노드들도 클러스터 이름은 동일하게하고 노드 이름만 `node-2`, `node-3`로 변경하겠습니다.

이제 3개의 노드들 모두 `bin`폴더의 `elasticsearch` 파일을 실행합니다.

```bash
$ cd bin
$ ./elasticsearch
```

그리고, curl로 확인해봅니다.

```bash
$ curl -XGET localhost:9200
{
  "name" : "node-1",
  "cluster_name" : "es-1",
  ...생략...
```

```bash
$ curl -XGET localhost:9201
{
  "name" : "node-2",
  "cluster_name" : "es-1",
  ...생략...
```

```bash
$ curl -XGET localhost:9202
{
  "name" : "node-3",
  "cluster_name" : "es-1",
  ...생략...
```

여기서 보면 포트를 수정하지 않았지만 기본적으로 9200번으로 설정이 됩니다. 그리고 노드가 추가 될때마다 1씩 증가하여 9201, 9202로 포트번호가 설정됩니다.

만약 포트번호를 바꾸고 싶을때는 `./config/elasticsearch.yml` 파일에서 `network`부분의 `port`를 수정하면 됩니다.

### 데이터 삽입

이름과 나이라는 데이터를 `node-1`에 삽입하도록 하겠습니다.

```bash
$ curl -XPUT localhost:9200/test/doc/1?pretty -H 'Content-Type: application/json' -d '{ "name": "taehong", "age": 27 }'
{
  "_index" : "test",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 1,
    "failed" : 0
  },
  "_seq_no" : 0,
  "_primary_term" : 1
}
```

`node-1`에 데이터를 정상적으로 삽입했습니다.

<i class="fas fa-feather-alt"></i> **Note** 도메인 뒤에 `pretty`를 붙이면 결과물이 보기좋은 형태로 출력됩니다. 보기 좋게 만들어주는 역할만 하기 때문에 적지않아도 됩니다.
{: .notice--info}

삽입한 데이터를 조회해보는데, 이때 `node-1`이 아닌 `node-2`에서 가져오도록 하겠습니다.

```bash
$ curl -XGET localhost:9201/test/doc/1?pretty
{
  "_index" : "test",
  "_type" : "doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 0,
  "_primary_term" : 1,
  "found" : true,
  "_source" : {
    "name" : "taehong",
    "age" : 27
  }
}
```

`node-1`에 넣었지만 `node-2`에서도 가져올 수 있는 이유는 모두 같은 클러스터(`es-1`)에 바인딩 되어있기 때문입니다.

## Reference

<a href="https://www.elastic.co" target="_blank">https://www.elastic.co</a>

<i class="far fa-laugh-wink"></i> **P.S** 긴 글 읽어주셔서 감사합니다. 잘못된 부분이나 추가적인 설명이 필요한 경우 댓글 또는 이메일로 알려주세요. 글을 가져가시게 되면 간단하게 출처 남겨주시면 감사하겠습니다.
{: .notice--success}
