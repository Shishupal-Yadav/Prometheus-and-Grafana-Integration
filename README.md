# Prometheus-and-Grafana-Integration
Hola, this project is integration of Grafana and Prometheus
# Why we need to integrate Prometheus and Grafana

![alt text](https://logz.io/wp-content/uploads/2017/03/prometheus-monitoring.jpg)

The combination of Prometheus and Grafana is becoming a more and more common monitoring stack used by DevOps teams for storing and visualizing time series data. Prometheus acts as the storage backend and Grafana as the interface for analysis and visualization.

Prometheus collects metrics from monitored targets by scraping metrics from HTTP endpoints on these targets. But what about monitoring Prometheus itself?

Like any server running processes on a host machine, there are specific metrics that need to be monitored such as used memory and storage as well as general ones reporting on the status of the service. Conveniently, Prometheus exposes a wide variety of metrics that can be easily monitored. By adding Grafana as a visualization layer, we can easily set up a monitoring stack for our monitoring stack.
# What is Prometheus?

![alt text](https://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Prometheus_software_logo.svg/1024px-Prometheus_software_logo.svg.png)

Prometheus is an open-source systems monitoring and alerting toolkit originally built at SoundCloud. Since its inception in 2012, many companies and organizations have adopted Prometheus, and the project has a very active developer and user community. It is now a standalone open source project and maintained independently of any company.
# Prometheus main features are:

a multi-dimensional data model with time series data identified by metric name and key/value pairs
PromQL, a flexible query language to leverage this dimensionality
no reliance on distributed storage; single server nodes are autonomous
time series collection happens via a pull model over HTTP
pushing time series is supported via an intermediary gateway
targets are discovered via service discovery or static configuration
multiple modes of graphing and dashboarding support

# What is Grafana?

![alt text](https://xebialabs.com/wp-content/uploads/files/tool-chest/grafana.jpg)

Grafana is open source visualization and analytics software. It allows you to query, visualize, alert on, and explore your metrics no matter where they are stored. In plain English, it provides you with tools to turn your time-series database (TSDB) data into beautiful graphs and visualizations.

# Grafana main features are:
1.Dashboard templating
2.Provisioning
3.Annotations
4.Alerting and alert hooks
5.Monitoring your monitoring

# Project Description
Integrate Prometheus and Grafana and perform in following way:
1.  Deploy them as pods on top of Kubernetes by creating resources Deployment, ReplicaSet, Pods or Services
2.  And make their data to be remain persistent 
3.  And both of them should be exposed to outside world

# Let,s start our project

