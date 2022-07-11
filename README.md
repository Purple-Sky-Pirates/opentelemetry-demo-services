# Opentelemetry Docker-compose / kubernetes playground

It is what it is.

# Docker Compose

In order to fix the permissions on the config file, the following needs to be run (on fedora anyway):

```
chcon -Rt svirt_sandbox_file_t ./otel-collector-config.yaml
```