# EFK
                                             ELK/EFK

E-ElasticSearch:
    Search engine/database, used for storing logs
L/F- logstash/fluentd:
    Log importer and processor
K- kibana
    Visual GUI for the collected data

It's a tool which combines Elasticsearch, Fluentd and Kibana to manage logs. In which Elasticsearch is basically a NOSQL database that is used for search  logs and Fluentd uses to collect logs and send to Elasticsearch This latter will receive the logs and save it on Database. And the Kibana will fetch the logs from Elastic search and will display it on a web app. 


Why EFK?

When data constantly flows into the system, it will grow. As it grows larger, analytics will slow-up and in conclusion sluggish insights,which is likely to be a serious problem. So, there is a solution “EFK” which makes it way easier and faster to search and analyze large data sets.


The Majors tools of EFK are as follows:-

Elasticsearch :- It is a distributed, free and open search and analytics engine for all types of data, including textual, numerical, geospatial, structured, and unstructured.


Fluentd :- Fluentd allows you to unify data collection and consumption for a better use and understanding of data.


Kibana :- It is an free and open frontend application that sits on top of the Elastic Stack, providing search and data visualization capabilities for data indexed in Elasticsearch. 


 Features of ELK/EFK:

 System and application performance
 Logging
 Dashboards & visualizations




How to install and setup EFK: 
EFK components get deployed as follows,
Fluentd:- Deployed as a daemonset as it needs to collect the container logs from all the nodes. It connects to the Elasticsearch service endpoint to forward the logs.
Elasticsearch:- Deployed as statefulset as it holds the log data. We also expose the service endpoint for Fluentd and kibana to connect to it.
Kibana:- Deployed as deployment and connects to elasticsearch service endpoint.
 
Step 1:- $ Start minikube

 
 
 
Step 2: Create an service file for elastic :

Then apply the yaml file:-
$ Kubectl apply -f file-svc.yaml
 
 
 
 
Step 3:- Let’s create the ElasticSearch statefulset now :

Then apply the yaml file :-
$ Kubectl apply -f file-sts.yaml




Step 4:- Verifying ElasticSearch Deployment:

$ kubectl port-forward <pod-name> 9200:9200 -n <namespace>
Out will be like:-


To check health of the elasticsearch:-
$ curl http://localhost:9200/_cluster/health/?pretty









Step 5 : Create a manifest fluentd-role.yaml








Then apply the yaml file:-


$ kubectl apply -f fluentd-role.yaml












Step 6:  let’s create a service.yaml for fluentd








Then apply the yaml file:-


$ kubectl apply -f  fluentd-sa.yaml












Step 7 : let’s create a manifest for cluster rolebinding:






Then apply the yaml file:- 


$ kubectl apply -f fluentd-rb.yaml














Step 8: let’s deploy the daemonset now :





Then apply the yaml file-:

$ kubectl apply -f fluentd-ds.yaml
Step 9 : let’s create test-pod.yaml for verify the fluentd installation:







Then apply the yaml file :-


$kubectl apply -f  test-pod.yaml













Step  10:- Now we are going to Deploy kibana deployment and service

Create the file- dep.yaml




Then apply the yaml file:-


$ kubectl apply -f file-dep.yaml






Step 11:- Now let’s create a service of type NodePort to access Kibana UI:





Then apply the yaml file:


$ kubectl apply -f file-svc.yaml











Step 12 :- Verifying Kibana Deployment

$ kubectl port-forward <kibana-pod-name> 5601:5601 -n <namespace>
After this access the UI through the browser
$ curl http://localhost:5601/app/kibana


When your “Dashboard” is ready now you can configure your Kibana and check whether the logs are being picked up by fluentd and stored in elasticsearch or not(You can also use the sample logs/metrics/graphs and various logs available there).





