# kube authentication resources
openid
we canuse identity provider but openid easer to use.
onelogin not free .
Add oidc setup to kops cluster:

```
spec:
  kubeAPIServer:
    oidcIssuerURL: https://ammarqqqq.auth0.com/
    oidcClientID: cbOppGNkaVsB5yzYazr6fZ6P50ba3B0m
    oidcUsernameClaim: sub
```
 
kops update cluster dev.psamman.com --state=s3://kops-state-ammar --yes
Create UI:

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/kops/master/addons/kubernetes-dashboard/v1.6.3.yaml
```

on auth0-deployment change the domain name on the file


on auth0-secrets.yml put the secret key copy it from auth0 domain
l0D8XKkQRbiR9gJ1eaZBsOg9uH4NFGImm7kNiG3PvAxmh1LOhgcDZ0OMFVr4d7U8

decode it
echo -n "l0D8XKkQRbiR9gJ1eaZBsOg9uH4NFGImm7kNiG3PvAxmh1LOhgcDZ0OMFVr4d7U8" |base64

bDBEOFhLa1FSYmlSOWdKMWVhWkJzT2c5dUg0TkZHSW1tN2tOaUczUHZBeG1oMUxPaGdjRFowT01GVnI0ZDdVOA==



tobe sure its use decode 
echo "bDBEOFhLa1FSYmlSOWdKMWVhWkJzT2c5dUg0TkZHSW1tN2tOaUczUHZBeG1oMUxPaGdjRFowT01G
VnI0ZDdVOA==" |base64 --decode
AUTH0_CLIENT_SECRET: bDBEOFhLa1FSYmlSOWdKMWVhWkJzT2c5dUg0TkZHSW1tN2tOaUczUHZBeG1oMUxPaGdjRFowT01G
VnI0ZDdVOA== # enter the auth0 secret here

on the file auth0-deployment on this lines
- name: AUTH0_CONNECTION
            value: Username-Password-Authentication # auth0 user database connection

            we need go to auth0.com and create database connection
            on the name its need matching the name on the file
            name: Username-Password-Authentication

            and enbale this connection to the client
            go to clients
            kuberntes client
            connections
            and enable Username-Password-Authentication

            the code it self on the folder ../kubernetes-auth-server
            on route53 create new record name authserver.dev.psamman.com cname the lbalancer

            now goto authserver.dev.psamman.com
            enter the username and password we created on auth0.com
            now go to kubernetes-auth-server folder
            
            now make sure python3 installed
            install depeneniec on requirements-cli.txt
            apt install python3-pip

            sudo pip3 install -r ../kubernetes-auth-server/requirements-cli.txt
            
            AUTH0_CLIENT_ID=cbOppGNkaVsB5yzYazr6fZ6P50ba3B0m AUTH0_DOMAIN=ammarqqqq.auth0.com APP_HOST=authserver.dev.psamman.com //home/ammar/Desktop/study_guide/kubernetes/udemy/advanced-kubernetes/advanced-kubernetes-course/kubernetes-auth-server/cli-auth.py

            alias kubectl="kubectl --token=\$(AUTH0_CLIENT_ID=cbOppGNkaVsB5yzYazr6fZ6P50ba3B0m AUTH0_DOMAIN=ammarqqqq.auth0.com APP_HOST=authserver.dev.psamman.com   /home/ammar/Desktop/study_guide/kubernetes/udemy/advanced-kubernetes/advanced-kubernetes-course/kubernetes-auth-server/cli-auth.py)"

            https://kubernetes.io/docs/reference/access-authn-authz/authentication/