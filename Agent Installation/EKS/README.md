# Installing Datadog Daemonset into EKS
- This set up will enable the following in Datadog for EKS:
    - Enable Datadog agent to accept APM traces on port 8126
    - log collection from containers stdout/stderr and stream it to Datadog.
    - Enable Live Process Collection in Datadog

<details>
<summary>Click to toggle for steps</summary>

- **Obtain API Key and APP Key from Datadog Platform**
    - [API Key](https://docs.datadoghq.com/account_management/api-app-keys/#add-an-api-key-or-client-token)
    - [APP Key](https://docs.datadoghq.com/account_management/api-app-keys/#add-application-keys)

- **Replace it in [values-eks.yaml](https://github.com/jon94/eval-dd-poc/blob/main/Agent%20Installation/EKS/values-eks.yaml)**

- **Create Namespace** 
```
kubectl create ns datadog
```

- **Create Daemonset and necessary resources using helm**
```
helm repo add datadog https://helm.datadoghq.com
helm repo update
helm install datadog datadog/datadog -n datadog -f values-eks.yaml
```

</details>

# Validate installation
<details>
<summary>Click to toggle for steps</summary>

- **Check Daemonset** 
    - Should see daemonsets count match your node counts.   
    - If it does not match, then you might have taints set on your nodes. We will then need to add tolerations in the helm values file. Refer to [values-eks.yaml](https://github.com/jon94/eval-dd-poc/blob/main/Agent%20Installation/EKS/values-eks.yaml) for more information.
```
kubectl get ds -n datadog
```
</details>