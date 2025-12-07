🎺 Instrumenting Applications With OpenTelemetry
In this module, you'll take your first hands-on steps into OpenTelemetry instrumentation.
You will learn how to enable auto-instrumentation for Java and Python so that traces are generated without modifying any code, and observe these traces in the already-running Collector logs.

By the end of this module, you'll understand how auto-instrumentation works and how to use it in practice.

🐳 Navigating the Lab Environment
Before we begin, let's take a quick tour of the lab environment:

Overview Tab: You are here! This is your starting point, with detailed information on the lab's topic. Make sure to review it thoroughly before moving on to the tasks.

Tasks Tab: When you're ready to put your knowledge to the test, switch to the Tasks tab. You'll find a mix of multiple-choice questions and hands-on tasks that will challenge the effectiveness of the prompts you craft.

Toggle Panel Size: Need to access the Terminal? Just click the Toggle Panel Size button at the top (next to the timer).

🤖 What Is Instrumentation?
Instrumentation is how your application produces telemetry data.

There are two approaches:

Manual Instrumentation: You explicitly add API calls in your code to create spans and propagate context.
Auto-Instrumentation: An agent or package automatically hooks into common frameworks, libraries, and drivers to generate spans without touching the application code.
In this module, we focus on auto-instrumentation.

☕️ Auto-Instrumenting Java Applications
OpenTelemetry provides a Java agent for automatic instrumentation.

Steps for instrumentation:

Download the Java agent from https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/latest/download/opentelemetry-javaagent.jar.
Run your service with the agent:
java -javaagent:<path_to_agent_jar> -Dotel.service.name=<service_name> -Dotel.exporter.otlp.endpoint=<endpoint_address> -Dotel.exporter.otlp.protocol=<endpoint_protocol> -jar <target_jar_file>
The agent then automatically instruments frameworks (HTTP servers, clients, database drivers, etc.) and sends spans to the Collector.

🐍 Auto-Instrumenting Python Applications
OpenTelemetry provides a Python instrumentation package that works with frameworks like Flask, Django, and SQL libraries.

Steps for instrumentation:

Install the auto-instrumentation packages:
opentelemetry-api
opentelemetry-sdk
opentelemetry-instrumentation-flask
opentelemetry-exporter-otlp
opentelemetry-distro
opentelemetry-instrumentation-requests
Set environment variables:
OTEL_PYTHON_AUTO_INSTRUMENTATION_ENABLED
OTEL_EXPORTER_OTLP_INSECURE
OTEL_LOG_LEVEL
Run your application with instrumentation:
opentelemetry-instrument --traces_exporter <exporting_protocol> --exporter_otlp_endpoint=<endpoint_address> --service_name=<service_name> python <python_code_file>
The opentelemetry-instrument command automatically wraps supported libraries and frameworks, generating spans sent to the Collector.

🔎 Observing Traces
Once the application runs:

Spans are automatically generated for HTTP requests, database calls, and other common operations
Spans are sent via OTLP to the Collector
You can see the traces in the Collector logs (trace IDs, span names, attributes, timestamps, and parent/child relationships)
This lets you verify auto-instrumentation is working without modifying any application code.

📌 Conclusion
Instrumentation is how telemetry enters your system. Auto-instrumentation saves development time and provides instant observability. Java and Python apps can be instrumented with one agent/command. Traces then flow from the application -> SDK -> Collector automatically.


