# Local SD-Core Helm Charts 
Helm charts for the Sdcore deployment. This contains both control plane and user plane network functions

# Testing sd-core-helm-charts locally

1. Clone the repo 
```
git clone https://github.com/NgKore47/sdcore-helm-charts
```
> **Note:** See the git diff [here](./patch/sdcore-helm-charts.patch)


2. Make sure that the `sd-core-helm-charts` folder is present at this path `/home/ubuntu/core/sd-core-helm-charts`

3. Make sure to follow [this](./Pulling_private_images_using_Secrets.md) readme for `Pulling private docker images using Secrets`

4. Build helm dependency

```bash
cd cord/sdcore-helm-charts/sdcore-helm-charts/
helm dependency build   # or [helm dep update] to Update Helm dependencies
```


5. Clone Aether-in-a-Box

```bash
cd ~
git clone "https://gerrit.opencord.org/aether-in-a-box"
cd ~/aether-in-a-box
```
###  Method 1

6. Use this [sd-core-5g-values.yaml](./sd-core-5g-values.yaml) file. 

> **Note:** You can also see the difference between this `sd-core-5g-values.yaml` and the default one [here](./patch/aether-in-a-box.patch)

7. To install the 5G SD-CORE from the local charts:

```bash
make 5g-test
```

8. To verify the installation:

```bash
kubectl get pods -n omec 
```

###  Method 2

6. Prepare a kubernetes node to run 5g-core

```bash
make node-prep
```

7.  Install 5G using SD-Core umbrella helm chart:

The following command will deploy the SD-Core helm chart with release name sdcore-5g in the sdcore-5g namespace.

```bash
cd cord/sdcore-helm-charts/sdcore-helm-charts/
helm install -n sdcore-5g --create-namespace -f values.yaml sdcore-5g  ~/cord/sdcore-helm-charts/sdcore-helm-charts
```

8. To verify the installation:

```bash
xxxx@node:~$ helm -n sdcore-5g ls
NAME        NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
sdcore-5g   sdcore-5g       1               2022-03-05 16:25:32.338495035 -0700 MST deployed        sd-core-0.13.2
xxxx@node:~$

xxxx@node:~$ kubectl get pods -n sdcore-5g
NAME                          READY   STATUS    RESTARTS   AGE
amf-6cddb6ff5-g5kwp           1/1     Running   0          8d
ausf-64fb5c5df5-g9xps         1/1     Running   0          8d
gnbsim-0                      1/1     Running   0          8d
mongodb-0                     1/1     Running   0          8d
mongodb-1                     1/1     Running   0          8d
mongodb-arbiter-0             1/1     Running   0          8d
nrf-69794885b-pgl8f           1/1     Running   0          8d
nssf-fc9c48c89-dxqn7          1/1     Running   0          8d
pcf-5c7d7767c9-wv6dl          1/1     Running   0          8d
simapp-669b99db9d-lbbm4       1/1     Running   0          8d
smf-b87fc6f8f-4jdqr           1/1     Running   0          8d
smf-b87fc6f8f-xt2n2           1/1     Running   0          8d
udm-f948b57dc-n5b4h           1/1     Running   0          8d
udr-698445bd87-8ptpm          1/1     Running   0          8d
upf-0                         5/5     Running   0          8d
upf-adapter-c4844b7fb-wqbvw   1/1     Running   0          8d
webui-8cfb9659c-hqfp9         1/1     Running   0          8d
xxxx@node:~$
```

