# kube authorization with auth0

Add oidc setup to kops cluster:

create kops

kops create cluster --name=dev.psamman.com --state=s3://kops-state-ammar --zones=eu-west-1a --node-count=2 --node-size=t2.small --master-size=t2.small --dns-zone=dev.psamman.com --authorization RBAC
or 
edit cluster
spec:
  authorization:
  rbac: {}

kops edit cluster dev.psamman.com --state=s3://kops-state-ammar 

add the specs

```
spec:
  kubeAPIServer:
    oidcIssuerURL: https://ammarqqqq.auth0.com/
    oidcClientID: cbOppGNkaVsB5yzYazr6fZ6P50ba3B0m
    oidcUsernameClaim: name
    oidcGroupsClaim: http://authserver.dev.psamman.com/claims/groups
  authorization:
    rbac: {}

```
kops update cluster dev.psamman.com --state=s3://kops-state-ammar --yes

now go to auth0.com to extensions
find authorization extentions
install it
create a new group name developers
now assignuser to this groupmthen enable group and publish it.

now to go rules on the main page,create rule empy rulename it add group to token


Auth0 rule for groups

```
function (user, context, callback) {
  var namespace = 'http://authserver.dev.psamman.com/claims/'; // You can set your own namespace, but do not use an Auth0 domain

  // Add the namespaced tokens. Remove any which is not necessary for your scenario
  context.idToken[namespace + "permissions"] = user.permissions;
  context.idToken[namespace + "groups"] = user.groups;
  context.idToken[namespace + "roles"] = user.roles;
  
  callback(null, user, context);
}
```
now save this
now go to authentication adn run the scripts 
kubectl create -f ../authentication/ .
make sure that the loadbalance presented to route53 authserver.dev.psamman.com
now login on http://authserver.dev.psamman.com with your username u must take a token

now we are goingto kuberentes to give access to group developers to make them access 

on config file remove the user to test it

alias kubectl="kubectl --token=\$(AUTH0_CLIENT_ID=cbOppGNkaVsB5yzYazr6fZ6P50ba3B0m AUTH0_DOMAIN=ammarqqqq.auth0.com APP_HOST=authserver.dev.psamman.com   /home/ammar/Desktop/study_guide/kubernetes/udemy/advanced-kubernetes/advanced-kubernetes-course/kubernetes-auth-server/cli-auth.py)"

kubectl get nodes
username: ss
password: ss
forbidden no access
we need to run the rbac script to authorized us

goto role.yml
run kubectl get pods
now we can see the pod


