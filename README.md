# Opentelemetry Docker-compose / kubernetes playground

It is what it is.

Quarkus not using opentelemetry for logs yet....

https://github.com/quarkusio/quarkus/issues/9916


# Docker Compose

In order to fix the permissions on the config file, the following needs to be run (on fedora anyway):

```
chcon -Rt svirt_sandbox_file_t ./otel-collector-config.yaml
chcon -Rt svirt_sandbox_file_t ./data/loki/data
chcon -Rt svirt_sandbox_file_t ./data/loki/tmp
chcon -Rt svirt_sandbox_file_t ./loki.yaml
chcon -Rt svirt_sandbox_file_t ./loki-local-config.yaml


```
# Get Log CLI


```
curl -O -L "https://github.com/grafana/loki/releases/download/v2.6.0/logcli-linux-amd64.zip"
unzip "logcli-linux-amd64.zip"
chmod a+x "logcli-linux-amd64"
sudo mv logcli-linux-amd64 /usr/local/bin/logcli
```

Use LogCLI:

```
logcli labels
```

Push data to LOKI

LOKI requires:

```
{
  "streams": [
    {
      "stream": {
        "label": "value"
      },
      "values": [
          [ "<unix epoch in nanoseconds>", "<log line>" ],
          [ "<unix epoch in nanoseconds>", "<log line>" ]
      ]
    }
  ]
}
```

Push something to LOKI to test:

```
currenttime=$(date +%s%N)

curl -v -H "Content-Type: application/json" -XPOST -s "http://localhost:3100/loki/api/v1/push" --data-raw '{"streams": [{ "stream": { "foo": "bar2" }, "values": [ [ "1657562000750679106", "fizzbuzz" ] ] }]}'
```

