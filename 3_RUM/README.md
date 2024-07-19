# Read first

## Setup RUM via NPM

https://docs.datadoghq.com/real_user_monitoring/browser/setup/#npm

- Ensure that sessionSampleRate is set to 20 and sessionReplaySampleRate is set to 0.

<details>
<summary>Click to toggle for steps</summary>
```
import { datadogRum } from '@datadog/browser-rum'

datadogRum.init({
  applicationId: '<DATADOG_APPLICATION_ID>',
  clientToken: '<DATADOG_CLIENT_TOKEN>',
  // `site` refers to the Datadog site parameter of your organization
  // see https://docs.datadoghq.com/getting_started/site/
  site: '<DATADOG_SITE>',
  //  service: 'my-web-application',
  //  env: 'production',
  //  version: '1.0.0',
  sessionSampleRate: 20,
  sessionReplaySampleRate: 0,
  trackResources: true,
  trackLongTasks: true,
  trackUserInteractions: true,
  enablePrivacyForActionName: true,
});

```
</details>

##Advanced Configuration
- https://docs.datadoghq.com/real_user_monitoring/browser/advanced_configuration/?tab=npm