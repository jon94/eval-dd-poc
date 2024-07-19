# Read this
- [Unified Service Tagging](https://docs.datadoghq.com/getting_started/tagging/unified_service_tagging/?tab=kubernetes#overview) powers the correlation capability of Datadog. 
- [Line 5-7](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/Java/sample-deployment.yaml#L5-L7) and Line [39-41](https://github.com/jon94/eval-dd-poc/blob/main/2_APM%20Instrumentation/Java/sample-deployment.yaml#L39-L41)
    - These are the tags you have to add into your application deployment YAML manifest.
    - Datadog's [Admission Controller](https://docs.datadoghq.com/getting_started/tagging/unified_service_tagging/?tab=kubernetes#configuration:~:text=If%20you%20deployed,Controller%20documentation.) will help to propagate these tags as env variables later on once APM SDK is injected. 
- APM Library Injection uses Admission Controller to help inject init-containers pre your pod's lifecycle so you can easily instrument your applications.
    - We will be using the [local library injection method](https://docs.datadoghq.com/tracing/trace_collection/library_injection_local/?tab=kubernetes#remove-apm-for-all-services-on-the-infrastructure) via the admission controllers
    - This allows us to have tighter control of versions for languages that have EOL support (E.g. NodeJS)