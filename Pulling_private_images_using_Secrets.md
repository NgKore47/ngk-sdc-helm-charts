### Pulling private images using Secrets

1. Create kubernetes secrets using command line
```bash
kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=ngkore --docker-password=dckr_pat_YpE3gKTQhmnQtk4vnT3y0OhoZHI --docker-email=ngkore47@gmail.com
```

2. After creating secret, lets create [private-docker-images.yml](./private-docker-images.yml) which uses private images:

3. Create a pod which uses private images:
```bash
kubectl create -f private-docker-images.yml
```



