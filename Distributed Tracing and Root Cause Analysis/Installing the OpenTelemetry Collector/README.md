# Lab: Installing and Running the OpenTelemetry Collector

In this lab, you downloaded, extracted, and ran the **OpenTelemetry Collector (Contrib edition)**. This collector will act as the central pipeline for processing and exporting telemetry signals from our instrumented applications.

---

## 📥 Step 1 — Download the Collector

Use `wget` to download the `.deb` package for the Linux AMD64 OpenTelemetry Collector (v0.139.0):

```bash
wget https://github.com/open-telemetry/opentelemetry-collector-releases/releases/download/v0.139.0/otelcol-contrib_0.139.0_linux_amd64.deb
```

---

## 📦 Step 2 — Extract the Collector Contents

Instead of installing the package system-wide, we extract it locally:

```bash
dpkg -x otelcol-contrib_0.139.0_linux_amd64.deb .
```

This gives you:

```
./usr/bin/otelcol-contrib
./etc/otelcol-contrib/config.yaml
...
```

You can now run the collector directly from this folder—no system install needed.

---

## 🏃 Step 3 — Run the Collector

Navigate to the directory where the collector and your `config.yaml` file reside:

```bash
cd /root/code
```

Then run:

```bash
./usr/bin/otelcol-contrib --config config.yaml
```

---

## 📡 What You Should See

When the collector starts successfully, you’ll see live logs similar to:

```
2025-12-07T12:35:37.589-0500 info  Starting otelcol-contrib... {"service.version": "0.139.0"}
2025-12-07T12:35:37.590-0500 info  Starting extensions... 
2025-12-07T12:35:37.590-0500 info  Starting zpages extension
2025-12-07T12:35:37.590-0500 info  Starting pprof extension
2025-12-07T12:35:37.591-0500 info  Starting GRPC server {"endpoint": "[::]:4317"}
2025-12-07T12:35:37.591-0500 info  Starting HTTP server {"endpoint": "[::]:4318"}
2025-12-07T12:35:37.591-0500 info  Everything is ready. Begin running and processing data.
```

This confirms that:

* The Collector has started
* Extensions are running (zPages, pprof, health_check)
* OTLP receivers are listening on ports:

  * **4317 (gRPC)**
  * **4318 (HTTP)**

Your collector is now ready to receive telemetry.

---

## ⚠️ But Why Don’t We See Any Trace Data Yet?

Even though the Collector is running, **our application is not sending any trace data to it yet**.

Why?

Because the application is **not instrumented**.

To generate and export telemetry, an application must:

1. Use the OpenTelemetry SDK or auto-instrumentation
2. Initialize a tracer
3. Start spans
4. Export traces to the Collector via OTLP (4317 / 4318)

Right now, the Collector is ready and waiting—but the application hasn’t been taught to emit telemetry.

That’s what we’ll solve next.


