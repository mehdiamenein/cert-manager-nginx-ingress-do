# Cert-Manager & Nginx-Ingress

This instruction is using the release versions below (base on the time you are reading this document, newer versions might be available):

**Nginx-Ingress:** v1.1.0 (https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.0/deploy/static/provider/cloud/deploy.yaml)

**Cert-Manager:**  v1.0.4 (https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.yaml)

**Kubernete:** 1.21.5-do.0 

<a href="https://www.digitalocean.com/?refcode=6ce89bef9635&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge"><img src="https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg" alt="DigitalOcean Referral Badge" /></a>

***
# Commands

```bash
# Installing cert-manager
kubectl.exe apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.0.4/cert-manager.yaml

# Checking if all components of cert-manager namespace are ready
kubectl -n cert-manager get all

# Creating the ingress namespace
kubectl create ns ingress-nginx

# Install nginx-ingress in its respective namespace
kubectl -n ingress-nginx apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.1.0/deploy/static/provider/cloud/deploy.yaml

# First install the issuer (if you want, you can modify the issuer to be an staging one)
kubectl apply -f .\issuer.yaml

# Check your issuer
kubectl describe clusterissuer letsencrypt-production

# Add your application (here we are using a non-presisted ghost instance)
    # Deployment
kubectl apply -f .\ghost-deployment.yaml
    # Cluster IP
kubectl apply -f .\ghost-cluster.yaml
# Please make sure all pods are running
kubectl get pods
# When the pods are active please provision an instance of ingress
kubectl apply -f .\ingress-kube.yaml
# Your ingress will be seeking the certificate but it does not exist so the next step is creation of that file
kubectl apply -f .\certificate.yaml

# Check if your certificate is issued successfully
# (in this case we are using the same domain name for the name of certificate as well)
kubectl.exe describe certificate {your-domain}
```

# References

This repo is heavily inspired by marcel.dempers repo and youtube video below **(Many Thanks)**:
- https://github.com/marcel-dempers/docker-development-youtube-series/tree/master/kubernetes/cert-manager
- https://www.youtube.com/watch?v=hoLUigg4V18

Further information about ghost cms:
https://ghost.org/

Further information on cert-manager and ingress-nginx: 
- https://cert-manager.io/docs/
- https://kubernetes.github.io/ingress-nginx/