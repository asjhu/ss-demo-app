## BeyondTrust Secrets-Agent side car example
#### Docs:  https://www.beyondtrust.com/docs/beyondinsight-password-safe/ps/integrations/kubernetes-secrets-agent/index.htm
### Description
#### In app-deploy.yml, the side car line 10 runs alongside application line 50, retrieves the target directory "aks" (line 45) and writes the entire contents to files in a shared volume at /run/secrets (lines 22-24).  The application displays the content of rmq01_id and rmq01_key (lines 52-53 ./src/index.php).  In this case "TS-k8s" is a parent directory making the full path of secrets files /run/secrets/TS-k8s/aks
### Setup
#### 1. Create an API registration in BeyondInsight (does not require a user password).
#### 2. Create or use an existing Secrets Safe group. Group name becomes parent directory (TS-k8s) in our example
#### 3. Add Secrets Safe feature to group from step 2.  Assign either Full Control or Read permission 
#### 4. Create or use an existing BeyondInsight user
#### 5. Add API registration to the group from step 2
#### 6. Add the user to the group from step 2
#### 7. Create a child directory "aks" in our example
#### 8. Create 2 secrets of type Credential in TS-k8s/aks/rmq01_id and rmq01_key in our example
#### 9. Clone repo
```sh
 git clone https://github.com/asjhu/ss-demo-app.git
 ```
 #### 10. Build docker image
 ```sh
 docker build -t phpapp:V1 .
```
#### 11. In your K8s cluster, create secrets called "ss-creds" with 2 data secrets called "apiendpoint" and "clientsecret" referenced in lines 34-43. apiendpoint=https://PasswordSafeInstance.com:443/BeyondTrust/api/public/v3   clientsecret="<API Key>;runas=username;" API key created in step 1 and username created in step 4.  Don't forget to base64 encode
#### 12: Apply app-deploy.yml
```sh
kubectl apply -f app-deploy.yml
```
#### 13. Line 70 is nodePort = 30380, this is the TCP port you need to connect.
