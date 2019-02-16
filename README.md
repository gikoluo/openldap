# README


## Setup
docker-compose up

## Initialation
1. Test: 
`ldapsearch -x -W -D 'cn=admin,dc=luochunhui,dc=com' -b "" -s base`
```
Enter LDAP Password:
# extended LDIF
#
# LDAPv3
# base <> with scope baseObject
# filter: (objectclass=*)
# requesting: ALL
#
#
dn:
objectClass: top
objectClass: OpenLDAProotDSE
# search result
search: 2
result: 0 Success
# numResponses: 2
# numEntries: 1
```

2. Import Template data
ldapadd -x -W -D 'cn=admin,dc=gkewang,dc=cn' -f org_template.ldif


3. Use ApacheDirectoryStudio to setup the groups and users.
Hostname: 127.0.0.1
Port: 389
Encryption method: No encryption
Provider: Apache Directory LDAP Client API
BindDN or user: cn=admin,dc=luochunhui,dc=com
Bind password: **password set in docker-compose**




Create StartSSL

follow: https://www.stevejenkins.com/blog/2016/06/create-a-free-ssl-certificate-with-startssl/

openssl req -newkey rsa:2048 -keyout ldap.gkewang.com.key -out ldap.gkewang.com.csr



## Deploy Service

kubectl apply -f openldap-etc-pvc.yaml
kubectl apply -f openldap-lib-pvc.yaml
kubectl apply -f openldap-deployment.yaml
kubectl apply -f openldap-service.yaml


kubectl expose deployment nginx — port 80 — target-port 80 — name nginx

kubectl expose deployment openldap --type=LoadBalancer --name=openldap


kubectl get deployments openldap
kubectl get services openldap
kubectl describe services openldap
kubectl get pods 

kubectl port-forward openldap-588c59d575-6wvz4  8389:389



## Connect to ldap
1. connect ldap with ApacheDirectoryStudio
PORT: 8389
DN: cn=admin,dc=ldap,dc=gkewang,dc=com
Bind Pwd: XXXXX

2. Load org_template.ldif to ldap



## Deploy by Helm 
helm install --name openldap -f helm.yaml stable/openldap


openssl req \
  -new \
  -newkey rsa:4096 \
  -days 3650 \
  -nodes \
  -x509 \
  -subj "/C=US/ST=CA/L=SF/O=Docker-demo/CN=app.example.org" \
  -keyout app.example.org.key \
  -out app.example.org.cert