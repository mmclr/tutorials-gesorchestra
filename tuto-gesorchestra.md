# **Tuto :** Deploy georchestra with docker , on production 

Here, we are going to explain how to adapt the docker composition offered by thje Georchestra Community, to run it on production/ 

This is probably still not really a production-ready deployment, but will work on a remote server, with a custom domain name and ssl certificate 

## **Step 1 :** Clone the official repo

```bash
git clone --recurse-submodules https://github.com/georchestra/docker.git
cd docker
git checkout 23.0 && git submodule update
```

## **Step 2 :** Change traefik configuration file 

We will add a CertificateResolver :

*resources/traefik_custom.yaml :*
```yaml
global: 
  sendAnonymousUsage: false
  checkNewVersion: false

entryPoints:
  web:
    address: ":80"
    http:
      redirections:
        entryPoint:
          to: websecure
          scheme: https

  websecure:
    address: ":443"

providers:
  docker:
    watch: true
    exposedByDefault: false
    endpoint: unix:///var/run/docker.sock

api:
  dashboard: true

log:
  level: INFO

certificatesResolvers:
  letsEncrypt:
    acme:
      # comment this line when going to production
      caServer: https://acme-staging-v02.api.letsencrypt.org/directory
      email: jean.pommier@pi-geosolutions.fr
      storage: /acme/acme.json
      httpChallenge:
        entryPoint: web

ping: {}
```