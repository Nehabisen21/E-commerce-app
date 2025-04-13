## Monitoring of EKS cluster, kubernetes components and workloads using prometheus and grafana using HELM
- <p id="Monitor">Install Helm Chart on Ubuntu</p>
```bash
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```
```bash
chmod 700 get_helm.sh
```
```bash
./get_helm.sh
```

#
-  Add Helm Stable Charts for Your Local Client
```bash
helm repo add stable https://charts.helm.sh/stable
```

#
- Add Prometheus to the Helm Repository to setup prometheus via HELM
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
```

#
- Create Prometheus Namespace for isolation
```bash
kubectl create namespace prometheus
```
```bash
kubectl get namespace
```

#
- Above we have added prometheus repository, now we will install Prometheus using Helm
```bash
helm install stable prometheus-community/kube-prometheus-stack -n prometheus
```

#
- Verify prometheus installation
```bash
kubectl get pods -n prometheus
```

#
- Check the services file of the Prometheus
```bash
kubectl get svc -n prometheus
```

#
- Expose Prometheus and Grafana to the external world through Node Port
> [!Important]
> change the type from Cluster IP to NodePort after changing make sure you save the file and open the assigned nodeport to the service.

```bash
kubectl edit svc stable-kube-prometheus-sta-prometheus -n prometheus
```

#
- Verify service
```bash
kubectl get svc -n prometheus
```

#
- Now,let’s change the SVC file of the Grafana and expose it to the external world
> [!Important]
> change the type from Cluster IP to NodePort after changing make sure you save the file and open the assigned nodeport to the service.
```bash
kubectl edit svc stable-grafana -n prometheus
```


#
- Check grafana service
```bash
kubectl get svc -n prometheus
```

#
- Get a password for grafana
```bash
kubectl get secret --namespace prometheus stable-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```
> [!Note]
> Username: admin

#
- Now, view the Dashboard in Grafana
![image](https://github.com/user-attachments/assets/d2e7ff2f-059d-48c4-92bb-9711943819c4)
![image](https://github.com/user-attachments/assets/647b2b22-cd83-41c3-855d-7c60ae32195f)
![image](https://github.com/user-attachments/assets/cb98a281-a4f5-46af-98eb-afdb7da6b35a)
