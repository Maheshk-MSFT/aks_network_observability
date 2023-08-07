
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

<img width="767" alt="a1" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/1f2eea79-ea25-4e3e-875b-ac2b69f9133b">

<img width="801" alt="a2" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/399605bd-88af-4921-955e-aef884582358">

<img width="754" alt="a3" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/f99d5051-d833-474c-b188-a7714c3b96f4">

<img width="759" alt="a4" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/266a2dc9-33f0-4b4e-b961-a0c397dfb8f5">

<img width="824" alt="a45" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/04af26f5-3748-45d4-8c4e-c17097e14e39">

<img width="817" alt="a5" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/0909c611-1680-4151-bd89-8cf47fadfc6a">

<img width="820" alt="a6" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/19f60f1f-1baa-47fe-b451-cb313e49e8ca">

<img width="821" alt="a7" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/676c9e78-442b-490c-9feb-bca8aa9893ba">

<img width="824" alt="a8" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/e251f2da-c1ca-4b96-87e2-d77e3d63c8e6">

<img width="825" alt="a9" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/0eb254d6-6df4-4a66-a3ea-9f11b9961823">

<img width="828" alt="s9" src="https://github.com/Maheshk-MSFT/aks_network_observability/assets/61469290/a7e96558-9acb-42b9-bdf9-c401ea5327b8">












