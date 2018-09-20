# Advanced Kubernetes course
## logging
* These files are modified from https://github.com/kubernetes/kubernetes/blob/master/cluster/addons/fluentd-elasticsearch to add persistent storage and kibana plugins

## Steps
* kubectl create -f logging/
* Run a cluster with sufficient resources (3x t2.medium at least on AWS)
* Make sure to label nodes with beta.kubernetes.io/fluentd-ds-ready=true


kops create cluster --name=dev.psamman.com --state=s3://kops-state-ammar --zones=eu-west-1a --node-count=3 --node-size=t2.medium --master-size=t2.micro --dns-zone=dev.psamman.com

k



kops create secret --name dev.psamman sshpublickey admin -i ~/.ssh/id_rsa.pub --state=s3://kops-state-ammar

kubectl label nodes ip-172-20-34-125.eu-west-1.compute.internal  beta.kubernetes.io/fluentd-ds-
ready=true

all nodes 

