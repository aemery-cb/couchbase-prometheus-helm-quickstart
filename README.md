# Setting up Prometheus monitoring for Couchbase Server with Helm

## Introduction

Quick start for setting up Prometheus monitoring with Couchbase Server with minimal configuration.

## Prerequisites
1. [helm](https://helm.sh/docs/intro/install/)
2. [A Kubernetes Cluster](https://kubernetes.io/docs/setup/)
3. [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Steps
0. `git clone https://github.com/aemery-cb/couchbase-prometheus-helm-quickstart.git` 
1. Add in the Couchbase Helm Charts
    `helm repo add couchbase https://couchbase-partners.github.io/helm-charts/`
2. Add in the Prometheus Community Helm Charts
    
    `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
3. And update helm
    
    `helm repo update`
4. Install Couhbase Autonomous Operator 
    
    `helm install couchbase -—values couchbase.yml couchbase/couchbase-operator`
5. Install Kube Prometheus Stack
    
    `helm install prometheus -—values prometheus.yml prometheus-community/kube-prometheus-stack`

6. Create a service for the couchbase metrics exporter

    `kubectl apply -f metric-service.yml`

6. Create the ServiceMonitor to pick up the newly created endpoint 
    
    `kubectl apply -f service-monitor.yml`

7. Port forward Grafana, Prometheus

    ```
    kubectl port-forward svc/prometheus-operated 9090:9090 # prometheus will be available at localhost:9090
    kubectl port-forward svc/prometheus-grafana 3000:80 # grafana will be available at localhost:3000 
    ```

The Couchbase Metrics Service will appear in Prometheus under `http://localhost:9090/targets`

Login credentials for Grafana dashboard at the time of writing are `admin/prom-operator`

To help provide server load and populate dashboards [pillowfight](https://blog.couchbase.com/performance-testing-load-testing-couchbase-pillowfight/) may be useful
