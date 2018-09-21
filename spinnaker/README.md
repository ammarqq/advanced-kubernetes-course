# Spinnaker build
*
kops create cluster --name=dev.psamman.com --state=s3://kops-state-ammar --zones=eu-west-1a --node-count=2 --node-size=t2.medium --master-size=t2.small --dns-zone=dev.psamman.com 

helm init to prepare kubenretes cluster
on spinnaker.yml we are going to use the public registy.


Make changes to spinnaker.yml
* helm install --name demo -f spinnaker.yml stable/spinnaker
we will get an error forbidden user to run it on namespaces to resolve it

created file name rbac-config.yaml
run it
<!-- $ kubectl create -f rbac-config.yaml
serviceaccount "tiller" created
clusterrolebinding "tiller" created
$ helm init --service-account tiller -->

kubectl create serviceaccount --namespace kube-system tiller
 # kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
  # kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
   
   now helm commandwillbe work

 helm install --name demo -f spinnaker.yml stable/spinnaker

 NAME:   demo
LAST DEPLOYED: Thu Sep 20 16:39:39 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Secret
NAME                     TYPE    DATA  AGE
demo-minio               Opaque  2     3m
demo-redis               Opaque  1     3m
demo-spinnaker-registry  Opaque  1     3m

==> v1/ConfigMap
NAME                           DATA  AGE
demo-minio                     2     3m
demo-spinnaker-halyard-config  3     3m

==> v1/ServiceAccount
NAME                    SECRETS  AGE
demo-spinnaker-halyard  1        3m

==> v1/Service
NAME                    TYPE       CLUSTER-IP     EXTERNAL-IP  PORT(S)   AGE
demo-minio              ClusterIP  None           <none>       9000/TCP  3m
demo-redis-master       ClusterIP  100.67.29.202  <none>       6379/TCP  3m
demo-spinnaker-halyard  ClusterIP  None           <none>       8064/TCP  3m

==> v1beta2/Deployment
NAME        DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
demo-minio  1        1        1           1          3m

==> v1/StatefulSet
NAME                    DESIRED  CURRENT  AGE
demo-spinnaker-halyard  1        1        3m

==> v1/PersistentVolumeClaim
NAME        STATUS  VOLUME                                    CAPACITY  ACCESS MODES  STORAGECLASS  AGE
demo-minio  Bound   pvc-9f476dc6-bcda-11e8-9970-0aa9507fe960  10Gi      RWO           gp2           3m

==> v1/ClusterRoleBinding
NAME                      AGE
demo-spinnaker-spinnaker  3m

==> v1/RoleBinding
NAME                    AGE
demo-spinnaker-halyard  3m

==> v1beta2/StatefulSet
NAME               DESIRED  CURRENT  AGE
demo-redis-master  1        1        3m

==> v1/Pod(related)
NAME                          READY  STATUS     RESTARTS  AGE
demo-minio-66c99c7cf-49sk8    1/1    Running    0         3m
demo-redis-master-0           1/1    Running    0         3m
demo-install-using-hal-b6kdc  0/1    Completed  0         3m
demo-spinnaker-halyard-0      1/1    Running    0         3m


NOTES:
1. You will need to create 2 port forwarding tunnels in order to access the Spinnaker UI:
  
  


2. Visit the Spinnaker UI by opening your browser to: http://127.0.0.1:9000

To customize your Spinnaker installation. Create a shell in your Halyard pod:

  kubectl exec --namespace default -it demo-spinnaker-halyard-0 bash

For more info on using Halyard to customize your installation, visit:
  https://www.spinnaker.io/reference/halyard/

For more info on the Kubernetes integration for Spinnaker, visit:
  https://www.spinnaker.io/reference/providers/kubernetes-v2/



when we run this commands 

export DECK_POD=$(kubectl get pods --namespace default -l "cluster=spin-deck" -o jsonpath="{.items[0].metadata.name}")
  kubectl port-forward --namespace default $DECK_POD 9000

  we can access it fromour localhost
  127.0.0.1:9000

  now we need to configure github and dockerhub

  https://github.com/ammarqq/docker-demo.git

  now we need to link githubwith dockerhub
  go to settings link account services and link it with github

  once it linked we can from create tab create an automated build 

  name it spinnaker-node-demo
  the name must be the same name on the yaml file image:
  note:
  ammarqqqq/spinnaker-node-demo on spinnaker.yml the same name on dockerhub.
u can use helm upgrade  demo -f spinnaker.yml stable/spinnaker to upgrade the new repository if u want
now goto build settings to manually trigger.

go to 127.0.0.1:9000 from localhost
and create application

now we need to create a loadbalancer
stack name is dev
port 80 target is 300 coz nodejs 

now on cluster create a service group
stack dev
on container select spinnaker-node-demo
loadbalancer will be nodejsdev
change the container port to 3000
add to probe and change the port to 3000
and create
kubectl get pods
u will see container nodejsdev creating
kubectl proxy

copy the link

now going to pipeline
create a pipeline
trigger deploy  to dev
automated triger from docker registry
user the image spinnaker-demo
andadd to stage
type deploy
stage name : deploy to dev
deploy configuration

add a server group
nodejs-dev
select add the info

one more stage
destroy a server group
click toggle for existing cluster
namespace is default 
cluster is nodejs-dev
target previouse server group
and save

lets create new branch
git branch -b v1.0.2
git checkout -b v1.0.2
change on the nodejs file
git commit -m "change to version2"
git push origin v1.0.2
it will send new branch to github


# Git and docker
* Setup git repository on github or bitbucket
* Setup docker hub account
* Link docker hub account with github or bitbucket
* Create new automated build

# Spinnaker configuration
* Create new application
* Create new loadbalancer
* Create new server group
* Create new pipeline

# Persistence
* Currently minio (an S3 compatible storage system) is providing persistence for Spinnaker
  * If you are on AWS, you might rather want to use S3 itself, but that doesn't seem to be possible yet with this chart
