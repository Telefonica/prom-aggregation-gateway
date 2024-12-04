# Contributing

## Install Go

https://golang.org/doc/install

## Install dependencies

```sh
go mod tidy
```

## Run

```sh
go run . --apiListen 127.0.0.1:8080
```

## Send some metrics

```
echo "some_metric 3.14" | curl --data-binary @- http://127.0.0.1:8080/metrics/job/some_job

printf "#TYPE another_metric gauge\nanother_metric 42\n" | curl --data-binary @- http://127.0.0.1:8080/metrics/job/some_job
```

## See your metric

Open http://127.0.0.1:8080/metrics in your browser or use `curl`:

```sh
curl http://127.0.0.1:8080/metrics
```

Expected result

```
# TYPE another_metric gauge
another_metric{job="some_job"} 42
# TYPE some_metric untyped
some_metric{job="some_job"} 3.14
```

## Simulate a scrape from Prometheus

-   With "Prometheus/1.0" as user-agent
-   Gauges will be cleared after the scrape

```sh
curl -H "User-Agent: Prometheus/1.0" http://127.0.0.1:8080/metrics
```

Returns the same as above. But if executed again, the `gauge` metrics are cleared:

```
# TYPE some_metric untyped
some_metric{job="some_job"} 3.14
```
