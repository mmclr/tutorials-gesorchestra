# Tuto : Deploy georchestra with docker , on production 

Here, we are going to explain how to adapt the docker composition offered by thje Georchestra Community, to run it on production/ 

This is probably still not really a production-ready deployment, but will work on a remote server, with a custom domain name and ssl certificate 

## Step 1 : clone the official repo

```
git clone --recurse-submodules https://github.com/georchestra/docker.git
cd docker
git checkout 23.0 && git submodule update
```

## Step 2 : 