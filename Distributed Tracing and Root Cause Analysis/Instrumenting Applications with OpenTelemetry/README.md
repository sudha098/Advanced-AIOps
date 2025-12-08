# OpenTelemetry Instrumentation Lab – Cheque Management System

[![OpenTelemetry](https://img.shields.io/badge/OpenTelemetry-Observability-blue)](https://opentelemetry.io/)

This repository demonstrates **auto-instrumentation** of a Java Spring Boot backend and a Python Flask frontend using OpenTelemetry. Traces, metrics, and logs are collected and sent to an OpenTelemetry Collector.

---

## Table of Contents

* [Overview](#overview)
* [Lab Environment](#lab-environment)
* [Instrumentation Concepts](#instrumentation-concepts)

  * [Manual vs Auto-Instrumentation](#manual-vs-auto-instrumentation)
* [Java Backend Instrumentation](#java-backend-instrumentation)
* [Python Frontend Instrumentation](#python-frontend-instrumentation)
* [Observing Traces and Metrics](#observing-traces-and-metrics)
* [Next Steps](#next-steps)

---

## Overview

In this lab, you will learn to:

* Enable **auto-instrumentation** for Java and Python applications
* Observe telemetry data (traces, metrics, logs) in the OpenTelemetry Collector
* Understand how spans are propagated between frontend and backend services

---

## Lab Environment

<details>
<summary>Click to expand</summary>

* **Overview Tab**: Read all background information before starting.
* **Tasks Tab**: Contains hands-on tasks and multiple-choice questions.
* **Toggle Panel Size**: Expand terminal as needed.

</details>

---

## Instrumentation Concepts

### Manual vs Auto-Instrumentation

<details>
<summary>Click to expand</summary>

* **Manual Instrumentation**: Explicitly add API calls to create spans and propagate context in your code.
* **Auto-Instrumentation**: An agent or package automatically instruments frameworks, libraries, and drivers without modifying code.

> This lab focuses on **auto-instrumentation**.

</details>

---

## Java Backend Instrumentation

<details>
<summary>Click to expand</summary>

1. Download the **OpenTelemetry Java agent**:

```bash
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar -P /opt/
```

2. Launch the Spring Boot backend (`chequebackend-0.0.1-SNAPSHOT.jar`) with the agent:

```bash
java -javaagent:/opt/opentelemetry-javaagent.jar \
     -Dotel.service.name=cheque-backend \
     -Dotel.exporter.otlp.endpoint=http://localhost:4317 \
     -Dotel.exporter.otlp.protocol=grpc \
     -jar /root/code/chequemgmt/backend/target/chequebackend-0.0.1-SNAPSHOT.jar
```

3. Backend is now auto-instrumented. Spans will be sent to the Collector.

</details>

---

## Python Frontend Instrumentation

<details>
<summary>Click to expand</summary>

1. Activate virtual environment:

```bash
cd ~
source venv/bin/activate
```

2. Install OpenTelemetry Python packages (already installed in lab):

```bash
pip install opentelemetry-api opentelemetry-sdk \
            opentelemetry-instrumentation-flask \
            opentelemetry-exporter-otlp \
            opentelemetry-distro \
            opentelemetry-instrumentation-requests
```

3. Set environment variables:

```bash
export OTEL_PYTHON_AUTO_INSTRUMENTATION_ENABLED=true
export OTEL_EXPORTER_OTLP_INSECURE=true
export OTEL_LOG_LEVEL=debug
export OTEL_TRACES_SAMPLER=always_on
```

4. Launch Flask frontend with instrumentation:

```bash
cd /root/code/chequemgmt/frontend/
opentelemetry-instrument \
      --traces_exporter otlp \
      --exporter_otlp_endpoint=localhost:4317 \
      --service_name=frontend-service \
      python3 app.py
```

> Keep both frontend and backend processes running for tracing.

</details>

---

## Observing Traces and Metrics

<details>
<summary>Click to expand</summary>

* Open another terminal to view the **Collector logs**:

```bash
journalctl -u otelcol-contrib -f
```

* You should see metrics, logs, and traces from both frontend and backend services.
* Frontend requests automatically trigger backend traces.

> Note: Large volumes of log entries make it difficult to follow end-to-end requests. Use a **trace visualization tool** in the next module to visualize spans, timings, and bottlenecks.

</details>

---

## Next Steps

* Set up **trace visualization** to see end-to-end request paths.
* Explore **trace sampling and filters** to control volume of telemetry data.
* Extend instrumentation to other services in your system.

---

✅ **Outcome:**
By completing this lab, you understand how **auto-instrumentation** works for Java and Python, how telemetry flows from application → SDK → Collector, and how to observe traces in practice.

---
