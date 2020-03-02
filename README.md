# rucio-k8s-tutorial

* install minikube
* install kubectl
* install helm: https://helm.sh/docs/intro/install/
* add repos:

      > helm repo add stable https://kubernetes-charts.storage.googleapis.com/
      > helm repo add rucio https://rucio.github.io/helm-charts

* enable ingress:

      > minikube addons enable ingress

* install postgres:

      > helm install postgres stable/postgresql -f postgres_values.yaml
      
* run init container:

      > kubectl apply -f init-pod.yaml 
      
* install server:

      > helm install server rucio/rucio-server -f server.yaml

* check cluster ingress ip and add to /etc/hosts:

      > kubectl get ing
        NAME                       HOSTS               ADDRESS          PORTS     AGE
        server-rucio-server        rucio-server.info   192.168.39.162   80, 443   30m
        server-rucio-server-auth   rucio-auth.info     192.168.39.162   80        30m
  
      > (editor) /etc/hosts
        192.168.39.162 rucio-server.info
        192.168.39.162 rucio-auth.info

* check if servers are running:

        > curl http://rucio-server.info/ping
          {"version": "1.21.10"}
        > curl http://rucio-auth.info/ping
          {"version": "1.21.10"}
