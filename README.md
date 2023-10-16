## Presto Environment

### About

This environment will allow you to run Presto (Using docker-compose).
 
It Includes the following:

   * MariaDB
   * Hive 3.x 
   * Presto

### Prerequisite
 
Update the following configuration files with your AWS credentials:

* `etc/s3.properties`
* `hive/conf/hdfs-site.xml`
* `hive/conf/hive-site.xml`

### Running

```sh
$ docker-compose up -d
```

### using Presto CLI

```sh
$ docker-compose exec presto presto-cli --catalog s3 --schema default
```

### Running Hive Server (optional)

```sh
$ docker-compose exec -d hive hiveserver2 
```

This will start Hive server in the background.

You could use `beeline` to connect to the server after it's up and running

```sh
$ docker-compose exec hive beeline -u jdbc:hive2://localhost:10000
```

CREATE EXTERNAL TABLE amazon_reviews_parquet(
marketplace string,
customer_id string,
review_id string,
product_id string,
product_parent string,
product_title string,
star_rating int,
helpful_votes int,
total_votes int,
vine string,
verified_purchase string,
review_headline string,
review_body string,
review_date int,
year int)
PARTITIONED BY (product_category string)
ROW FORMAT SERDE
'org.apache.hadoop.hive.ql.io.parquet.serde.ParquetHiveSerDe'
STORED AS INPUTFORMAT
'org.apache.hadoop.hive.ql.io.parquet.MapredParquetInputFormat'
OUTPUTFORMAT
'org.apache.hadoop.hive.ql.io.parquet.MapredParquetOutputFormat'
LOCATION
's3a://quickstart/parquet/';


mc config host add minio http://localhost:9000 admin password
presto:default>
select product_title,review_headline, total_votes from amazon_reviews_parquet where product_category='Video_Games' order by total_votes DESC limit 
presto:default> select product_title,review_headline, total_votes from amazon_reviews_parquet where product_category='Video_Games' order by total_votes DESC limit 10;


insert into amazon_reviews_parquet select 'test1','test2','test3','test4','test5','test6',1,2,3,'test1','test2','test3','test4',1,2023,'ac'


CREATE TABLE s3.hive_storage.sample_table (
   col1 varchar, 
   col2 varchar);