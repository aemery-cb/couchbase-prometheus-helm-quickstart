# Setting up Prometheus monitoring for Couchbase Server with Helm

## Introduction

Sometimes you just want a Couchbase Sever cluster with Monitoring via Prometheus and a Grafana dashboard... really quickly.

## Prerequisites
1. helm
2. A Kubernetes Cluster
3. kubectl

## Steps
0. git clone this repo
1. Add in the Couchbase Helm Charts
    
    `helm repo add couchbase https://couchbase-partners.github.io/helm-charts/`
2. Add in the Prometheus Community Helm Charts
    
    `helm repo add prometheus-community https://prometheus-community.github.io/helm-charts`
3. And update helm
    
    `helm repo update`
4. Install Couhbase Autonomous Operator 
    
    `helm install <my-release> —values couchbase.yml couchbase/couchbase-operator`
5. Install Kube Prometheus Stack
    
    `helm install <my-release> —values prometheus.yml prometheus-community/kube-prometheus-stack`
6. Create the ServiceMonitor required
    
    `kubectl apply -f monitor.yml`
