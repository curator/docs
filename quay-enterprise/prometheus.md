# Prometheus metrics under Quay Enterprise

Quay Enterprise exports a [Prometheus](https://prometheus.io/)-compatible endpoint on each instance to allow for easy monitoring and alerting.

## Exposing the prometheus endpoint

The Prometheus-compatible endpoint on the Quay Enterprise instance can be found at port `9092`. Simply add `-p 9092:9092` to the `docker run` command (or expose the port via the `Pod` configuration in Kubernetes).

## Setting up Prometheus to consume metrics

In order for Prometheus to pull all the metrics cluster-wide, it needs to be aware of the full set of Quay Enterprise instances running. In the typical setup, this is done by placing all the Quay Enterprise instances into a single named DNS entry, which is then given to Prometheus.

### DNS configuration under Kubernetes

Under Kubernetes, a simple [service](http://kubernetes.io/docs/user-guide/services/) can be configured to provide the DNS entry for Prometheus. Documentation on running Prometheus under Kuberentes can be found at [Prometheus and Kubernetes](https://coreos.com/blog/prometheus-and-kubernetes-up-and-running.html) and [Monitoring Kubernetes with Prometheus](https://coreos.com/blog/monitoring-kubernetes-with-prometheus.html).

### DNS configuration for a manual cluster

A good solution for creating this DNS-entry when not running under Kubernetes is [SkyDNS](https://github.com/skynetservices/skydns), which can operate on top of an [etcd](https://github.com/coreos/etcd) cluster. A manual cluster can add and remove entries in etcd corresponding to each instance in the cluster, which SkyDNS will automatically pickup and thus update the DNS entry for Prometheus.