# Read this
- [Unified Service Tagging](https://docs.datadoghq.com/getting_started/tagging/unified_service_tagging/?tab=kubernetes#overview) powers the correlation capability of Datadog. 
- [Line 5-7](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/sample-deployment.yaml#L5-L7) and Line [39-41](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/sample-deployment.yaml#L39-L41)
    - These are the tags you have to add into your application deployment YAML manifest.
    - Datadog's [Admission Controller](https://docs.datadoghq.com/getting_started/tagging/unified_service_tagging/?tab=kubernetes#configuration:~:text=If%20you%20deployed,Controller%20documentation.) will help to propagate these tags as env variables later on once APM SDK is injected. 
- APM Library Injection uses Admission Controller to help inject init-containers pre your pod's lifecycle so you can easily instrument your applications.
    - We will be using the [local library injection method](https://docs.datadoghq.com/tracing/trace_collection/library_injection_local/?tab=kubernetes#remove-apm-for-all-services-on-the-infrastructure) via the admission controllers
    - This allows us to have tighter control of versions for languages that have EOL support (E.g. NodeJS)
- [Line 25](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/Java/sample-deployment.yaml#L25) and [Line 37](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/Java/sample-deployment.yaml#L37) are the places you need to annotate and label the deployment manifest
    - Note that in the example, the application is running on Java. Hence on [Line 25](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/Java/sample-deployment.yaml#L25), you see admission.datadoghq.com/java-lib.version: v1.21.0.
    - Change the value accordingly by referring to the [different languages here](https://docs.datadoghq.com/tracing/trace_collection/library_injection_local/?tab=kubernetes#step-2---annotate-your-pods-for-library-injection)
    - Use the following datadog tracer version numbers for your languages.
        | Language  | DD Tracer Library Version                          |
        |-----------|----------------------------------------------------|
        | Java      | admission.datadoghq.com/java-lib.version: "v1.37.0" |
        | NodeJS14  | admission.datadoghq.com/js-lib.version: "v3.57.0"   |
        | NodeJS16  | admission.datadoghq.com/js-lib.version: "v4.42.0"   |
        | NodeJS18  | admission.datadoghq.com/js-lib.version: "v5.18.0"   |
        | Python2.9 | admission.datadoghq.com/python-lib.version: "v1.20.19" |
        | Python 3> | admission.datadoghq.com/python-lib.version: "v2.9.2" |
    - **Please sound off to us if you are using uwsgi or gevent on python.**
