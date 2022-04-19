# InfluxDB Cluster

[![CN doc](https://img.shields.io/badge/文档-中文版-blue.svg)](https://github.com/chengshiwen/influxdb-cluster/wiki)
[![EN doc](https://img.shields.io/badge/document-English-blue.svg)](https://github.com/chengshiwen/influxdb-cluster/wiki/Home-Eng)
[![LICENSE](https://img.shields.io/github/license/chengshiwen/influxdb-cluster.svg)](https://github.com/chengshiwen/influxdb-cluster/blob/master/LICENSE)
[![Releases](https://img.shields.io/github/v/release/chengshiwen/influxdb-cluster.svg)](https://github.com/chengshiwen/influxdb-cluster/releases)
![GitHub stars](https://img.shields.io/github/stars/chengshiwen/influxdb-cluster.svg?label=github%20stars&logo=github)
[![Docker pulls](https://img.shields.io/docker/pulls/chengshiwen/influxdb.svg)](https://hub.docker.com/r/chengshiwen/influxdb)

## An Open-Source, Distributed, Time Series Database

InfluxDB Cluster is an open source **time series database** with
**no external dependencies**. It's useful for recording metrics,
events, and performing analytics.

InfluxDB Cluster is inspired by [InfluxDB Enterprise](https://docs.influxdata.com/enterprise_influxdb/v1.8/), [InfluxDB v1.8.10](https://github.com/influxdata/influxdb/tree/v1.8.10) and [InfluxDB v0.11.1](https://github.com/influxdata/influxdb/tree/v0.11.1), aiming to replace InfluxDB Enterprise.

InfluxDB Cluster is easy to maintain, and can be updated in real time with upstream [InfluxDB 1.x](https://github.com/influxdata/influxdb/tree/master-1.x).

## Features

* Built-in [HTTP API](https://docs.influxdata.com/influxdb/latest/guides/writing_data/) so you don't have to write any server side code to get up and running.
* Data can be tagged, allowing very flexible querying.
* SQL-like query language.
* Clustering is supported out of the box, so that you can scale horizontally to handle your data. **Clustering is currently in production state.**
* Simple to install and manage, and fast to get data in and out.
* It aims to answer queries in real-time. That means every data point is
  indexed as it comes in and is immediately available in queries that
  should return in < 100ms.

## Clustering

> **Note**: The clustering of InfluxDB Cluster is exactly the same as that of InfluxDB Enterprise.

Please see: [Clustering in InfluxDB Enterprise](https://docs.influxdata.com/enterprise_influxdb/v1.8/concepts/clustering/)

Architectural overview:

![architecture](https://github.com/chengshiwen/influxdb-cluster/wiki/image/architecture.png)

Network overview:

![architecture](https://docs.influxdata.com/img/enterprise/1-8-network-diagram.png)

## Installation

We recommend installing InfluxDB Cluster using one of the [pre-built releases](https://github.com/chengshiwen/influxdb-cluster/releases).

Complete the following steps to install an InfluxDB Cluster in your own environment:

1. [Install InfluxDB Cluster meta nodes](https://docs.influxdata.com/enterprise_influxdb/v1.8/install-and-deploy/production_installation/meta_node_installation/)
2. [Install InfluxDB Cluster data nodes](https://docs.influxdata.com/enterprise_influxdb/v1.8/install-and-deploy/production_installation/data_node_installation/)

For more information about deployment, please review the above two steps or see [Deployment](https://github.com/chengshiwen/influxdb-cluster/wiki/Home-Eng#deployment).

> **Note**: The installation of InfluxDB Cluster is exactly the same as that of InfluxDB Enterprise.

## Docker Quickstart

Download [docker-compose.yml](./docker/quick/docker-compose.yml) in [docker/quick](./docker/quick)

```
docker-compose up -d
docker exec -it influxdb-meta-01 bash
influxd-ctl add-meta influxdb-meta-01:8091
influxd-ctl add-meta influxdb-meta-02:8091
influxd-ctl add-meta influxdb-meta-03:8091
influxd-ctl add-data influxdb-data-01:8088
influxd-ctl add-data influxdb-data-02:8088
influxd-ctl show
```

## Getting Started

### Create your first database

```
curl -XPOST "http://influxdb-data-01:8086/query" --data-urlencode "q=CREATE DATABASE mydb"
```

### Insert some data
```
curl -XPOST "http://influxdb-data-01:8086/write?db=mydb" \
-d 'cpu,host=server01,region=uswest load=42 1434055562000000000'

curl -XPOST "http://influxdb-data-02:8086/write?db=mydb" \
-d 'cpu,host=server02,region=uswest load=78 1434055562000000000'

curl -XPOST "http://influxdb-data-02:8086/write?db=mydb" \
-d 'cpu,host=server03,region=useast load=15.4 1434055562000000000'
```

### Query for the data
```
curl -G "http://influxdb-data-02:8086/query?pretty=true" --data-urlencode "db=mydb" \
--data-urlencode "q=SELECT * FROM cpu WHERE host='server01' AND time < now() - 1d"
```

### Analyze the data
```
curl -G "http://influxdb-data-02:8086/query?pretty=true" --data-urlencode "db=mydb" \
--data-urlencode "q=SELECT mean(load) FROM cpu WHERE region='uswest'"
```

## Documentation

* Read more about the [design goals and motivations of the project](https://docs.influxdata.com/enterprise_influxdb/v1.8/).
* Follow the [getting started guide](https://docs.influxdata.com/enterprise_influxdb/v1.8/introduction/getting-started/) to learn the basics in just a few minutes.
* Learn more about [clustering](https://docs.influxdata.com/enterprise_influxdb/v1.8/concepts/clustering/) and [glossary](https://docs.influxdata.com/enterprise_influxdb/v1.8/concepts/glossary/).

## Contributing

If you're feeling adventurous and want to contribute to InfluxDB Cluster, see our [CONTRIBUTING.md](./CONTRIBUTING.md) for info on how to make feature requests, build from source, and run tests.

## Licensing

See [LICENSE](./LICENSE) and [DEPENDENCIES.md](./DEPENDENCIES.md).

## Looking for Support?

- Email: chengshiwen@apache.org
- [GitHub Issues](https://github.com/chengshiwen/influxdb-cluster/issues)
- [Community & Communication](https://github.com/chengshiwen/influxdb-cluster/wiki/Home-Eng#community--communication)