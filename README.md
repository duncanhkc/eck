# ECK
 
This project elaborate how to deploy an eck in K8s. Write helm charts for them if needed in the future.
 
Note:  
1. ECK cannot be deployed as helm chart yet, please follow the steps below to deploy eck.
2. IMPORTANT: DO NOT use "kubectl delete es.yaml" or "kubectl replace --force es.yaml". The data will be lost.

## ECK Deploy steps

1. Create namespaces, eck-nfs, eck-elastic-system, eck-es and eck-kibana, via rancher UI if you want to use project feature in Rancher.

2. Deploy nfs in eck-nfs.
   ```bash
   helm install nfs-client {path_to_nfs_helm} -n eck-nfs
   ```

3. Make sure eck operator is running in elastic-system namespace.
   https://www.elastic.co/guide/en/cloud-on-k8s/master/k8s-install-helm.html
   ```bash
   helm repo add elastic https://helm.elastic.co
   helm repo update
   helm install elastic-operator elastic/eck-operator --version=1.3.1 -n eck-elastic-system
   ```

4. Deploy elastic search
   ```bash
   kubectl apply -f es.yaml -n eck-es
   
   # Get the password of user "elastic"
   PASSWORD=$(kubectl get secret -n eck-es elasticsearch-es-elastic-user -o go-template='{{.data.elastic | base64decode}}')
       
   # call es service
   curl -u "elastic:$PASSWORD" -k "https://{es-http-service-ip}:9200"
   ```
 
5. Deploy kibana.
   ```bash
   kubectl apply -f kibana.yaml -n eck-kibana
   ```

6. Set ingress for es and kibana.
   ```bash
   kubectl apply -f ingress-es.yaml,ingress-kibana.yaml
   ```
   
7. 使用瀏覽器瀏覽 https://linxta-raes.garmin.com 和 https://linxta-rakibana.garmin.com 。(帳密同 step 4)