These folders should create a working ELK stack + filebeat config using helm commands, all with TLS certs and everything.

NOTE: this deployment was thought for a local environment using an nginx ingress controller:

```
helm upgrade --install ingress-nginx ingress-nginx \
  --repo https://kubernetes.github.io/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```


Once the ingress controller has been installed, install ELK stack by simply following these commands (make sure to kubectl get pods each time to make sure previous step has been properly finalized):

```bash
cd elasticsearch
helm --upgrade install elasticsearch .
cd ../logstash
helm --upgrade install logstash .
cd ../kibana
helm --upgrade install kibana .
cd ../filebeat
helm --upgrade install filebeat .
```
This creates an ingress for kibana.

You may add the ingress to your /etc/hosts by appending this line at the end of the file:

```
127.0.0.1 kibana-example.local
```

Once deployment is completed, login by going to 

kibana-example.local on your browser

and then login using the username `elastic` and the password returned by the following command:

```
kubectl get secrets elasticsearch-master-credentials --output jsonpath="{.data.password}" | base64 --decode
```


