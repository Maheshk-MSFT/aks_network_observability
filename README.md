
# aks_network_observability
 The Grafana dashboard for Network Observability add-on for AKS.

Collect the network traffic data of your AKS cluster. 
https://learn.microsoft.com/en-us/azure/aks/network-observability-managed-cli?tabs=non-cilium
-> Network Observability enables a centralized platform for monitoring application and network health. 
-> Prometheus collects AKS Network Observability metrics, and Grafana visualizes them. 
-> Let's see how to enale  Network Observability add-on and use Azure managed Prometheus and Grafana to check in detail 

```
az feature register --namespace "Microsoft.ContainerService" --name "NetworkObservabilityPreview"

az feature show --namespace "Microsoft.ContainerService" --name "NetworkObservabilityPreview"

az provider register -n Microsoft.ContainerService

az group create --name testresourcegroup --location eastus

new: 
az aks create --name testakscluster --resource-group testresourcegroup --location eastus --generate-ssh-keys --network-plugin azure --network-plugin-mode overlay --pod-cidr 192.168.0.0/16 --enable-network-observability
	
existing cluster: 
az aks update --resource-group testresourcegroup --name testakscluster --enable-network-observability 

az resource create --resource-group testresourcegroup --namespace microsoft.monitor --resource-type accounts --name myAzureMonitor --location eastus --properties '{}'

az grafana create --name myGrafana --resource-group testresourcegroup 

grafanaId=$(az grafana show --name myGrafana --resource-group testresourcegroup --query id --output tsv)
azuremonitorId=$(az resource show --resource-group testresourcegroup --name myAzureMonitor --resource-type "Microsoft.Monitor/accounts" --query id --output tsv)

az aks update --name testakscluster --resource-group testresourcegroup --enable-azure-monitor-metrics --azure-monitor-workspace-resource-id $azuremonitorId --grafana-resource-id $grafanaId

az aks get-credentials --name testakscluster --resource-group testresourcegroup
```

The Grafana dashboard for Network Observability add-on for AKS.
https://grafana.com/grafana/dashboards/18814-kubernetes-networking/
ID: 18814

![Uploading a1.png…]()
![Uploading a2.png…]()
<img width="754" alt="a3" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/cae328ec-a10d-4347-a90c-8774413d31eb">
<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/e53adf35-7600-4b15-a3bf-78d1e5e9a832">
<img width="817" alt="a5" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/95da33e9-5eb1-45a3-b8de-de0283410da6">

