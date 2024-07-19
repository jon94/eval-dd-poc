# Pre-req
- Please note that the user running the commands need to have the correct RBAC permissions for SCC creation.
- Please whitelist [this list](https://ip-ranges.datadoghq.com/) of IP ranges.
    - Or refer to "Network Access Requirements" tab in the shared [google sheet](https://docs.google.com/spreadsheets/d/13GJAbG8OCjhiLAjzJN0VRgj0xdnuT-Lxigg_6O9BDnM/edit?gid=110767519#gid=110767519)

# Installing Datadog Daemonset into OpenShift
- This set up will enable the following in Datadog for OpenShift:
    - Enable Datadog agent to accept APM traces on port 8126
    - log collection from containers stdout/stderr and stream it to Datadog.
    - Enable Live Process Collection in Datadog

<details>
<summary>Click to toggle for steps</summary>

- **Obtain API Key and APP Key from Datadog Platform**
    - [API Key](https://docs.datadoghq.com/account_management/api-app-keys/#add-an-api-key-or-client-token)
    - [APP Key](https://docs.datadoghq.com/account_management/api-app-keys/#add-application-keys)

- **Replace it in [values-os.yaml](https://github.com/jon94/eval-dd-poc/blob/main/1_Agent%20Installation/Openshift/values-os.yaml)**
 - Put in a clusterName according to the requirements shown in values-os.yaml

- **Create Namespace** 
```
oc create ns datadog
```

- **Create Daemonset and necessary resources using helm**
```
helm repo add datadog https://helm.datadoghq.com
helm repo update
helm install datadog datadog/datadog -n datadog -f values-os.yaml
```

</details>

# Validate installation
<details>
<summary>Click to toggle for steps</summary>

- **Check Daemonset** 
    - Should see daemonsets count match your node counts.   
    - If it does not match, then you might have taints set on your nodes. We will then need to add tolerations in the helm values file. Refer to [values-os.yaml](https://github.com/jon94/eval-dd-poc/blob/main/1_Agent%20Installation/Openshift/values-os.yaml) for more information.
```
oc get ds -n datadog
```
- **Check mutatingwebhookconfiguration**
    - This is required for admisson controller capability. 
    - Refer to this [document](https://docs.datadoghq.com/containers/troubleshooting/admission-controller/?tab=helm#overview) for troubleshooting if required.
```
oc get mutatingwebhookconfiguration -n datadog
```
</details>