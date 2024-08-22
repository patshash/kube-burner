<h1>
  <img src="./assets/consul_logo.svg" align="left" height="46px" alt="Consul logo"/>
  <span>Kube Burner</span>
</h1>

# Overview

This is a fork of the [*Kube-Burner*](https://kube-burner.github.io) project. The goal of this is to stress a K8s and Consul deployment. There are two scenarios we are using, most important is a fork of the `api-intensive` scenario. We are running a cut-down version that does not attach Kube Secrets or ConfigMaps. Instead we will deploy a new Namespace, Pod and Service. 


# Prerequisites

Install the [*Kube-Burner tool*](https://github.com/kube-burner/kube-burner/tree/main)

Kubernetes Cluster with Consul Mesh installed.

Optional: K9s to monitor the expirement

Kube-Burner will use the existing Kubernetes Context where it is run.

## Run an Experiment

`kube-burner init -c api-intensive.yml`

Update the values in the `api-intensive.yml` as required to meet load requirements.
```yaml
  - name: api-intensive
    jobIterations: 100 # Create 100 namespaces, with a pod and service inside. 
    qps: 5 # Limit object creation to 5 per second
    burst: 2 # Add pods in increments of 2. 

    churn: true # Randomly delete and re-create namespaces and objects
    churnPercent: 50 # Churn 50% of namespaces
    churnDelay: 3s # Length to wait between churn period
    churnDuration: 10s # How long a job is churned for
    churnCycles: 5 # Complete 5 cycles before cleanup and exit.
```
[*Configuration Reference*](https://kube-burner.github.io/kube-burner/latest/reference/configuration/?h=churn#jobs)

Monitor progress with K9s

The purpose of the above metrics is to create, remove and re-create numerous services and pods to stress K8s. They will need to be tweaked to meet cluster sizing.


## Clean-Up

1. Output from Kube-burner will be output to a local file.
