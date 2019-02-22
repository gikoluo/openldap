# README


## Deploy by docker-composer
docker-compose up


## Deploy by K8S Service

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

kubectl port-forward openldap-XXXXXX-XXXX  8389:389

## Deploy by Helm 
helm install --name openldap -f helm.yaml stable/openldap


## Deploy by Yum 
yum install openldap-server
follow the instruction: https://linuxtechlab.com/openldap-complete-guide-install-configure


## Usages
1. Test: 
`ldapsearch -x -W -D 'cn=Manager,dc=greenlandfinancial,dc=com' -b "" -s base`
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
ldapadd -x -W -D 'cn=admin,dc=greenlandfinancial,dc=cn' -f org_template.ldif


3. Use ApacheDirectoryStudio to setup the groups and users.
Hostname: 127.0.0.1
Port: 389
Encryption method: No encryption
Provider: Apache Directory LDAP Client API
BindDN or user: cn=admin,dc=greenlandfinancial,dc=com
Bind password: **password set in docker-compose**


4. Load org_template.ldif to ldap by ApacheDirectoryStudio

5. User Management

6. Group Management


## Extends

Create StartSSL

follow: https://www.stevejenkins.com/blog/2016/06/create-a-free-ssl-certificate-with-startssl/

openssl req -newkey rsa:2048 -keyout ldap.gkewang.com.key -out ldap.gkewang.com.csr



openssl req \
  -new \
  -newkey rsa:4096 \
  -days 3650 \
  -nodes \
  -x509 \
  -subj "/C=US/ST=CA/L=SF/O=Docker-demo/CN=app.example.org" \
  -keyout app.example.org.key \
  -out app.example.org.cert



