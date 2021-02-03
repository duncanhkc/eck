# Garmin ECK Helm Chart

## Prerequisite

1. Create namespaces, elastic-system and eck, via rancher UI if you want to use project feature in Rancher.

2. Make sure eck operator is running in elastic-system namespace. Recommend using helm chart to install this.
   https://www.elastic.co/guide/en/cloud-on-k8s/master/k8s-install-helm.html
   ```bash
   helm repo add elastic https://helm.elastic.co
   helm repo update
   helm install elastic-operator elastic/eck-operator -n elastic-system
   ```

## Configuration