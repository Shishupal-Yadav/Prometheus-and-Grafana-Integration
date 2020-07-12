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
**Step 1:**
First , by this single file I have created Persistent Volume which will be used by Prometheus as a permanent storage, so no data will be lost  even after rebooting.
Next in the same file I have created a service for exposing our prometheus using NodePort , this will expose our prometheus to outside world.
In last portion of file I have created a deployment for prometheus , using the recreate concept , it will automatically again do the deployment if our prometheus goes down.
I have used the prometheus pre created iamge from docker hub

*Here is the code for creating all the stuff mentioned above*

>apiVersion: v1
kind: PersistentVolumeClaim
metadata:
    name: prometheus-pvc
    labels:
        name: prometheuspvc
spec:
   accessModes:
    - ReadWriteOnce
   resources:
     requests:
        storage: 10Gi
---
apiVersion: v1
kind: Service
metadata:
  name: prometheus-service
  labels:
    app: prom-service
spec:
  selector:
    app: prometheus
  type: NodePort
  ports:
    - nodePort: 30002
      port: 9090
      targetPort: 9090
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: prometheus-deploy
  labels:
    app: prometheus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
      tier: monitoring
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: prometheus
        tier: monitoring
    spec:
      containers:
      - image: vimal13/prometheus
        name: prom-cont
        ports:
        - containerPort: 9090
          name: prom-cont
        volumeMounts:
        - name: prometheus-persistent-storage
          mountPath: /data
      volumes:
      - name: prometheus-persistent-storage
        persistentVolumeClaim:
          claimName: prometheus-pvc

         
         
**Step 2** Similarly for Grafana I have created a single file for Grafana , first I have created Persistent Volume , then I created Service and exposed graphana to outside world
and atlast I have created a deployment 
*Here is code for all the stuff mentioned above*
> apiVersion: v1
  kind: PersistentVolumeClaim
  metadata:
     name: grafana-pvc
     labels:
         name: grafanapvc
  spec:
    accessModes:
     - ReadWriteOnce
    resources:
      requests:
         storage: 10Gi
---
  apiVersion: v1
  kind: Service
  metadata:
    name: grafana-service
     labels:
      app: graf-service
  spec:
   selector:
    app: grafana
  type: NodePort
  ports:
    - nodePort: 30001
      port: 3000
      targetPort: 3000
---
apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: grafana-deploy
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
      tier: visualization
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: grafana
        tier: visualization
    spec:
      containers:
      - image: vimal13/grafana
        name: graf-cont
        ports:
        - containerPort: 3000
          name: graf-cont
        volumeMounts:
        - name: grafana-persistent-storage
          mountPath: /var/lib/grafana
      volumes:
      - name: grafana-persistent-storage
        persistentVolumeClaim:
          claimName: grafana-pvc
          
**Step 3** At last I have created a kustomization file , it will run the   above file automatically  in a sequence(we have written in kustomization file)  rather than doing it manually.

>apiVersion: kustomize.config.k8s.io/v1beta1
 kind: Kustomization

 resources:
   - prometheus.yml
   - grafana.yml
We just  have to run only one command:
**kubectl apply -k .**

***This command should be run in the folder, where this file is present using Command Prompt**
![GitHub Logo](/Images/2.jpg)


*Here our pods created*


![Pods](/Images/3.jpg)


*Now here type minikubeIP:Port(seperately for grafana and prometheus) in your browser we will get Grafana and Prometheus output as:

![Grafana](/Images/Grafana.jpg)

![Prometheus](/Images/prometheus.jpg)


*At last I created one dashboard as an example using some query*
![Integ](/Images/Prom-integ.jpg)
         
         
         
