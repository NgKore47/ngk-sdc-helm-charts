# Local SD-Core Helm Charts 
Helm charts for the Sdcore deployment. This contains both control plane and user plane network functions

# Testing sd-core-helm-charts locally

1. Clone this repo using:
```bash
git clone https://github.com/NgKore47/sdcore-helm-charts
```

2. Extract the `cord` folder at home i.e `/home/ubuntu/`
```bash
cd ~/
tar -xvzf ~/sdcore-helm-charts/cord.tar
```

3. Clone SD-Core repo using:
```bash
git clone https://github.com/NgKore47/SD-Core.git
cd ~/SD-Core
rm sd-core-5g-values.yaml
cp ~/sdcore-helm-charts/sd-core-5g-values.yaml .

# Configure the external IP of ngap in sd-core-5g-values.yaml
vim sd-core-5g-values.yaml

# Prepare your k8s clsuter
sudo apt install make
make node-prep
```

> **NOTE:** You can see the changes of `sd-core-5g-values.yaml` [here](./patch/sd-core-5g-values.patch)

4. Pulling private images using Secrets

- Create kubernetes secrets using command line
```bash
kubectl create secret docker-registry regcred --docker-server=https://index.docker.io/v1/ --docker-username=ngkore --docker-password=dckr_pat_3aEGHh5fOR7GYqCc9gB0_rvt5aw --docker-email=ngkore47@gmail.com
```

- After creating secret, you can check [private-docker-images.yml](./private-docker-images.yml) which uses private images:

- Create a pod which pull private images:

```bash
kubectl create -f ~/sdcore-helm-charts/private-docker-images.yml
```
> **Note:** After this step, wait for a few minutes to pull the private images

5. Deploy 5g-core
```bash
ENABLE_GNBSIM=false DATA_IFACE=<iface_ip> CHARTS=local make 5g-core
```

> **NOTE:** Here we are using local helm charts by specifying `CHARTS=local` in the above cmd. Even if we don't specify anything i.e remove `CHARTS=local` from above line, then also it will deploy 5g-core using local hekm charts. Also change `DATA_IFACE` according to your system.

6. After all the pods are up and running, run the following command on your Sd-Core Server:
```bash
sudo iptables -t nat -A POSTROUTING -o eno1 -j MASQUERADE
```
> **_NOTE:_** `eno1` is the local IP of the Server

7. Verify the installation:

```txt
xxxx@node:~$ kubectl get pods -n omec
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

<hr>

### Testing with gNB:

- Refer this file to test with **SDR(USRP B210)** [here](https://github.com/NgKore47/SD-Core-DPDK/blob/NgKore-sdc-dpdk-v1.0/usrp_conf/sd-core-dpdk-n78-b210.conf)

Or

- Refer this file to test with **External Radio(Liteon)** [here](https://github.com/NgKore47/Disaggregated_RAN_Core/blob/SD-Core-Disaggregated/mwc_20899_newfhi_E_3450.conf)
