<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/04a26a01-bb14-4d7a-9332-f87bb706e9c1">
<img width="754" alt="a3" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/7699027f-60c1-42f8-ada5-c7ebc5d919e3">
<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/5acaea71-25bf-462c-b2e3-a95bde49c4b8">
![Uploading a3.png…]()
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

![Uploading a2.png…]()
![Uploading a1.png…]()

<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/2b1ee2af-0abf-42a1-80fa-e6c48b7effd4">
![Uploading a3.png…]()
<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/f2d6d820-65bb-4475-9f89-59a9af6cd2f3">

