# Garmin ECK Helm Chart

WARNING: 
1. ECK is cannot be deployed as helm chart yet, please follow the steps below to deploy eck.
2. IMPORTANT: DO NOT use "kubectl delete" or "kubectl replace --force". The data will lose.

## ECK Deploy steps

1. Create namespaces, eck-nfs, eck-elastic-system, eck-es and eck-kibana, via rancher UI if you want to use project feature in Rancher.

2. Deploy nfs in eck-nfs.
   ```bash
   helm install nfs-client {path_to_nfs_helm} -n eck-nfs
   ```

3. Make sure eck operator is running in elastic-system namespace. Recommend using helm chart to install this.
   https://www.elastic.co/guide/en/cloud-on-k8s/master/k8s-install-helm.html
   ```bash
   helm repo add elastic https://helm.elastic.co
   helm repo update
   helm install elastic-operator elastic/eck-operator -n eck-elastic-system
   ```

3. Make sure that nfs-client storage exists. Then apply es.yaml.
   ```bash
   kubectl apply -f es.yaml -n es
   ```
   
4. 