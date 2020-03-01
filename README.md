# fluentd-http-s3

A template service for batching JSON objects through HTTP to AWS S3 using [fluentd](https://www.fluentd.org/).

### 1) build 

```console
docker build -t adrianulbona/fluentd-http-s3:latest .
```
### 2) configure

1) put your AWS S3 credentials in `.env` (use `.env.template` as a reference)
2) (Optional) tweak the fluentd config from `conf/fluent.conf`

### 3) run

```console
docker run --rm -p 9880:9880 -v ${PWD}/conf:/fluentd/etc \
    --env-file .env adrianulbona/fluentd-http-s3:latest
```

### 4) publish records

```console
curl -X POST -d 'json={"blob":"ceci n`est pas une pipe"}' http://localhost:9880/events
curl -X POST -d 'json={"blob":"it's been emotional"}' http://localhost:9880/events
curl -X POST -d 'json={"blob":"elementary, my dear Watson"}' http://localhost:9880/events
```

### 5) check batches

```console
aws s3 ls --recursive s3://fluentd-blackhole
2020-03-01 18:25:03         66 logs/year=2020/month=03/day=01/2020030117_0.json.gz
2020-03-01 18:26:02         47 logs/year=2020/month=03/day=01/2020030117_1.json.gz
```