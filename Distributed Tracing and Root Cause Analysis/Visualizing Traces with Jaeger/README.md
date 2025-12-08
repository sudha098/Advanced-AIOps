# Jaeger Tracing Lab

Visualize distributed traces from instrumented Java and Python services using **OpenTelemetry Collector** and **Jaeger**.

---

## Overview

This lab guides you through:

1. Running Jaeger All-In-One
2. Configuring OpenTelemetry Collector to export traces to Jaeger
3. Viewing application traces in the Jaeger UI

---

## Prerequisites

* Linux environment
* Instrumented services producing OpenTelemetry spans
* OpenTelemetry Collector installed and running
* `wget`, `tar`, and a text editor

---

## Step 1: Install Jaeger

```bash
cd ~
wget https://github.com/jaegertracing/jaeger/releases/download/v1.75.0/jaeger-1.75.0-linux-amd64.tar.gz
tar -xvzf jaeger-1.75.0-linux-amd64.tar.gz
```

Run Jaeger:

```bash
/root/jaeger-1.75.0-linux-amd64/jaeger-all-in-one \
  --collector.otlp.enabled=true \
  --collector.otlp.grpc.host-port=:14251 \
  --collector.otlp.http.host-port=:14252
```

* UI: [http://localhost:16686](http://localhost:16686)

> ⚠️ Jaeger v1 is deprecated. Consider v2 for production.

---

## Step 2: Configure OpenTelemetry Collector

Edit `/opt/otel/etc/otelcol-contrib/config.yaml`:

```yaml
exporters:
  debug:
    verbosity: detailed
  otlp/jaeger:
    endpoint: 0.0.0.0:14251
    tls:
      insecure: true

service:
  pipelines:
    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [debug, otlp/jaeger]
```

Restart the Collector:

```bash
systemctl restart otelcol-contrib
```

---

## Step 3: Generate and View Traces

1. Open the application frontend.
2. Perform actions like adding, refreshing, and deleting cheques.
3. Open Jaeger UI → **Search** → select `cheque-backend` or `frontend-service` to view traces.
<img width="1669" height="816" alt="image" src="https://github.com/user-attachments/assets/ebbbf5d2-a28f-4ced-861b-bb95817a0cc1" />

<img width="1897" height="839" alt="image" src="https://github.com/user-attachments/assets/e1912a86-63ed-48b8-b1b1-bff02dcd9b27" />

---

## Result

* All traces are now visible in Jaeger
* You can filter by service, operation, and trace duration
* Provides a clear visual representation of request flows

---

## References

* [Jaeger Documentation](https://www.jaegertracing.io/docs/latest/)
* [OpenTelemetry Collector Docs](https://opentelemetry.io/docs/collector/configuration/)

---

