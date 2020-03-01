# fluentd-http-s3

A template service for batching JSON objects through HTTP to AWS S3 using [fluentd](https://www.fluentd.org/).

### build 

```bash
docker build -t adrianulbona/fluentd-http-s3:latest .
```
### configure

1) put your AWS S3 credentials in `.env` (use `.env.template` as a reference)
2) (Optional) tweak the fluentd config from `conf/fluent.conf`

### run

```bash
docker run --rm -p 9880:9880 --env-file ${PWD}/conf:/fluentd/etc adrianulbona/fluentd-http-s3:latest
```

### publish records

```bash
curl -X POST -d 'json={"blob":"ceci n`est pas une pipe"}' http://localhost:9880/events
curl -X POST -d 'json={"blob":"this is the begining of a beautiful friendship"}' http://localhost:9880/events
curl -X POST -d 'json={"blob":"elementary, my dear Watson"}' http://localhost:9880/events
```

### check batches

```bash
aws s3 ls --recursive adrianulbona-playground
2020-03-01 18:25:03         66 logs/year=2020/month=03/day=01/2020030117_0.json.gz
2020-03-01 18:26:02         47 logs/year=2020/month=03/day=01/2020030117_1.json.gz
```