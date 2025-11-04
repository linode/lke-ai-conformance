# lke-ai-conformance

<img src="https://github.com/linode/lke-ai-conformance/blob/main/images/AkamaiCloud-Horizontal.png">
<img src="https://github.com/linode/lke-ai-conformance/blob/main/images/AkamaiCloud-HorizontalWhite.png">


Akamai Cloud LKE (Linode Kubernetes Engine) CNCF Kubernetes 1.34 AI Conformance

CNCF Kubernetes AI Conformance

https://docs.google.com/document/d/1hXoSdh9FEs13Yde8DivCYjjXyxa7j4J8erjZPEGWuzc/edit?tab=t.0


## Deploy LKE with APP Platform

* https://www.linode.com/docs/guides/deploy-llm-for-ai-inferencing-on-apl/#provision-an-lke-cluster

* https://techdocs.akamai.com/app-platform/docs/lke-automatic-install

## Install the NVIDIA GPU Operator

```
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia
helm repo update
helm install --wait --generate-name -n gpu-operator --create-namespace nvidia/gpu-operator --version=v25.10.0 --set dcgmExporter.serviceMonitor.enabled=true
```


## Verify GPU Operator

````
kubectl get pods -n gpu-operator
````

````
NAME                                                              READY   STATUS      RESTARTS   AGE
gpu-feature-discovery-gtfsf                                       1/1     Running     0          72s
gpu-feature-discovery-xs9wz                                       1/1     Running     0          72s
gpu-feature-discovery-z5qv2                                       1/1     Running     0          71s
gpu-operator-1762185269-node-feature-discovery-gc-8f698864lm44t   1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-master-86bbqzcxv   1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-7hkl2       1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-dg8n5       1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-ljv8j       1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-n4v7b       1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-nvcn4       1/1     Running     0          85s
gpu-operator-1762185269-node-feature-discovery-worker-x6rft       1/1     Running     0          85s
gpu-operator-596944c879-5ngk4                                     1/1     Running     0          85s
nvidia-container-toolkit-daemonset-4zqn8                          1/1     Running     0          71s
nvidia-container-toolkit-daemonset-b6j69                          1/1     Running     0          76s
nvidia-container-toolkit-daemonset-sm4sl                          1/1     Running     0          76s
nvidia-cuda-validator-4llgh                                       0/1     Completed   0          65s
nvidia-cuda-validator-4ntfc                                       0/1     Completed   0          46s
nvidia-cuda-validator-w65bs                                       0/1     Completed   0          47s
nvidia-dcgm-exporter-46gdf                                        1/1     Running     0          73s
nvidia-dcgm-exporter-gswfz                                        1/1     Running     0          73s
nvidia-dcgm-exporter-qd2xz                                        1/1     Running     0          71s
nvidia-device-plugin-daemonset-ptg8v                              1/1     Running     0          74s
nvidia-device-plugin-daemonset-v929s                              1/1     Running     0          72s
nvidia-device-plugin-daemonset-wq7jc                              1/1     Running     0          74s
nvidia-operator-validator-5465v                                   1/1     Running     0          71s
nvidia-operator-validator-7c54k                                   1/1     Running     0          75s
nvidia-operator-validator-zs8cg                                   1/1     Running     0          75s
user@tty-1aca4783-532c-46e4-a3de-fee9c279ed11:~$ 
````

## GPU detected and labeled
````
kubectl get node -o json | jq '.items[].metadata.labels'
````

````
 "kubernetes.io/arch": "amd64",
  "kubernetes.io/hostname": "lke529760-766837-0a634ab30000",
  "kubernetes.io/os": "linux",
  "lke.linode.com/pool-id": "766837",
  "node.k8s.linode.com/host-uuid": "40917c1f174b1ffae34d73e4febe42e76f5541ea",
  "node.kubernetes.io/instance-type": "g2-gpu-rtx4000a1-m",
  "nvidia.com/cuda.driver-version.full": "580.95.05",
  "nvidia.com/cuda.driver-version.major": "580",
  "nvidia.com/cuda.driver-version.minor": "95",
  "nvidia.com/cuda.driver-version.revision": "05",
  "nvidia.com/cuda.driver.major": "580",
  "nvidia.com/cuda.driver.minor": "95",
  "nvidia.com/cuda.driver.rev": "05",
  "nvidia.com/cuda.runtime-version.full": "13.0",
  "nvidia.com/cuda.runtime-version.major": "13",
  "nvidia.com/cuda.runtime-version.minor": "0",
  "nvidia.com/cuda.runtime.major": "13",
  "nvidia.com/cuda.runtime.minor": "0",
  "nvidia.com/gfd.timestamp": "1762185321",
  "nvidia.com/gpu-driver-upgrade-state": "upgrade-done",
  "nvidia.com/gpu.compute.major": "8",
  "nvidia.com/gpu.compute.minor": "9",
  "nvidia.com/gpu.count": "1",
  "nvidia.com/gpu.deploy.container-toolkit": "true",
  "nvidia.com/gpu.deploy.dcgm": "true",
  "nvidia.com/gpu.deploy.dcgm-exporter": "true",
  "nvidia.com/gpu.deploy.device-plugin": "true",
  "nvidia.com/gpu.deploy.driver": "pre-installed",
  "nvidia.com/gpu.deploy.gpu-feature-discovery": "true",
  "nvidia.com/gpu.deploy.node-status-exporter": "true",
  "nvidia.com/gpu.deploy.operator-validator": "true",
  "nvidia.com/gpu.family": "ampere",
  "nvidia.com/gpu.machine": "Compute-Instance",
  "nvidia.com/gpu.memory": "20475",
  "nvidia.com/gpu.mode": "graphics",
  "nvidia.com/gpu.present": "true",
  "nvidia.com/gpu.product": "NVIDIA-RTX-4000-Ada-Generation",
  "nvidia.com/gpu.replicas": "1",
  "nvidia.com/gpu.sharing-strategy": "none",
  "nvidia.com/mig.capable": "false",
  "nvidia.com/mig.strategy": "single",
  "nvidia.com/mps.capable": "false",
  "nvidia.com/vgpu.present": "false",
  "topology.kubernetes.io/region": "us-ord",
  "topology.linode.com/region": "us-ord"
````



## DRA Support

### Install Nvidia DRA driver
````
helm repo add nvidia https://helm.ngc.nvidia.com/nvidia && helm repo update
helm install nvidia-dra-driver-gpu nvidia/nvidia-dra-driver-gpu --namespace nvidia-dra-driver-gpu --create-namespace --values manifests/dra-driver-values.yaml
````

````
kubectl get deviceclasses
````

````
NAME                                        AGE
compute-domain-daemon.nvidia.com            4m32s
compute-domain-default-channel.nvidia.com   4m32s
gpu.nvidia.com                              4m32s
mig.nvidia.com                              4m32s
````


### Test DRA Resource Claims

````
kubectl apply -f manifests/resource-claim-template.yaml
kubectl apply -f manifests/dra-deployment.yaml
````

````
kubectl get resourceclaims 
kubectl get pods
````

Show DRA GPU Example
````
kubectl logs -f -lapp=dra-gpu-example
````
````
Tue Nov  4 17:24:18 UTC 2025
GPU 0: NVIDIA RTX 4000 Ada Generation (UUID: GPU-9d332149-daef-d8e3-1168-b247fd392cbe)
````

Show Resource Claim
````
kubectl get resourceclaims
````
````
NAME                                                STATE                AGE
dra-gpu-example-7b9b75dbb9-prgkm-single-gpu-mwr6m   allocated,reserved   74m
````

## AI Inference

### Gateway API in Istio

https://istio.io/latest/docs/setup/getting-started/#gateway-api

````
kubectl get crd gateways.gateway.networking.k8s.io &> /dev/null || \
{ kubectl kustomize "github.com/kubernetes-sigs/gateway-api/config/crd?ref=v1.3.0" | kubectl apply -f -; }
````

Pull down Istio files for demo
````
curl -L https://git.io/getLatestIstio | ISTIO_VERSION=1.27.3 sh -
````

Reset istio to use Gateway API

````
istio-1.27.3/bin/istioctl install --set profile=minimal -y
````

Apply sample app to show Gateway API operational
````
kubectl apply -f istio-1.27.3/samples/bookinfo/platform/kube/bookinfo.yaml
````
````
kubectl get services
kubectl get pods
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
````
````
<title>Simple Bookstore App</title>
````

Apply bookinfo gateway
````
kubectl apply -f samples/bookinfo/gateway-api/bookinfo-gateway.yaml
````
````
kubectl get gateway bookinfo-gateway
````
````
NAME               CLASS   ADDRESS          PROGRAMMED   AGE
bookinfo-gateway   istio   172.234.211.21   True         34m
````

## Gang Scheduling

Install Kueue

### Create namespace for kueue install

````
kubectl create ns  kueue-system
````

### Install via helm
````
helm install kueue oci://registry.k8s.io/kueue/charts/kueue --version=0.14.1 --namespace kueue-system --wait --timeout 300s
````

Wait until Ready
````
kubectl get deployments -n kueue-system
````

````
NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
kueue-controller-manager   1/1     1            1           63s
````

````
kubectl get pods -n kueue-system
````

````
NAME                                        READY   STATUS    RESTARTS      AGE
kueue-controller-manager-59957d6f7d-5sp8v   1/1     Running   2 (74s ago)   78s
````

Install resource Flavor
````
kubectl create namespace team-a
kubectl create namespace team-b
kubectl apply -f manifests/resource-flavor.yaml
````

Install Cluster Queue
````
kubectl apply -f manifests/cluster-queue.yaml
````

Install Local Queue
````
kubectl apply -f manifests/local-queue.yaml
````

Create jobs
````
kubectl create -f manifests/job-team-b.yaml
kubectl create -f manifests/job-team-b.yaml
````

````
k get jobs -A
````

````
NAMESPACE   NAME                      STATUS     COMPLETIONS   DURATION   AGE
team-a      sample-job-team-a-4k2df   Complete   3/3           16s        18s
team-b      sample-job-team-b-696zj   Complete   3/3           13s        32s
````

## Cluster Autoscaling

https://techdocs.akamai.com/cloud-computing/docs/manage-nodes-and-node-pools#autoscale-automatically-resize-node-pools


## Pod Autoscaling

Create Prometheus Rule

```shell
kubectl apply -f manifests/prometheus-rule.yaml
```

Install Prometheus Adapter

````
kubectl label servicemonitor -n gpu-operator gpu-operator prometheus=system
kubectl label servicemonitor -n gpu-operator nvidia-dcgm-exporter prometheus=system
helm install prometheus-adapter -n monitoring prometheus-community/prometheus-adapter --set prometheus.url="http://po-prometheus.monitoring.svc.cluster.local"
````

### Test Kubernetes Custom Metrics Endpoint

Check for the metrics exposed by DCGM Exporter such as `DCGM_FI_DEV_GPU_UTIL`

```
kubectl get --raw /apis/custom.metrics.k8s.io/v1beta1 | jq -r . | grep cuda_gpu
```

Create a deployment

```shell
kubectl apply -f manifests/cuda-deployment.yaml
```

### Create HPA to Scale deployment based on GPU Utilization

```shell
kubectl apply -f manifests/hpa.yaml
```

Simulate load on GPU

```shell
kubectl exec -it deployment/cuda -- bash

for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done & \
for (( c=1; c<=5000; c++ )); do ./vectorAdd; done &
```

## Accelerator Metrics

Show the DCGM metrics are installed and available

```
kubectl -n gpu-operator get svc
\NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
gpu-operator           ClusterIP   10.128.231.34   <none>        8080/TCP   9h
nvidia-dcgm-exporter   ClusterIP   10.128.20.33    <none>        9400/TCP   9h
```

```
kubectl port-forward service/nvidia-dcgm-exporter 9400:9400 -n gpu-operator
```

```
curl 127.0.0.1:9400/metrics
```

```
Handling connection for 9400
# HELP DCGM_FI_DEV_SM_CLOCK SM clock frequency (in MHz).
# TYPE DCGM_FI_DEV_SM_CLOCK gauge
DCGM_FI_DEV_SM_CLOCK{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 2310
# HELP DCGM_FI_DEV_MEM_CLOCK Memory clock frequency (in MHz).
# TYPE DCGM_FI_DEV_MEM_CLOCK gauge
DCGM_FI_DEV_MEM_CLOCK{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 9001
# HELP DCGM_FI_DEV_MEMORY_TEMP Memory temperature (in C).
# TYPE DCGM_FI_DEV_MEMORY_TEMP gauge
DCGM_FI_DEV_MEMORY_TEMP{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_GPU_TEMP GPU temperature (in C).
# TYPE DCGM_FI_DEV_GPU_TEMP gauge
DCGM_FI_DEV_GPU_TEMP{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 48
# HELP DCGM_FI_DEV_POWER_USAGE Power draw (in W).
# TYPE DCGM_FI_DEV_POWER_USAGE gauge
DCGM_FI_DEV_POWER_USAGE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 40.979000
# HELP DCGM_FI_DEV_TOTAL_ENERGY_CONSUMPTION Total energy consumption since boot (in mJ).
# TYPE DCGM_FI_DEV_TOTAL_ENERGY_CONSUMPTION counter
DCGM_FI_DEV_TOTAL_ENERGY_CONSUMPTION{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 455497156
# HELP DCGM_FI_DEV_PCIE_REPLAY_COUNTER Total number of PCIe retries.
# TYPE DCGM_FI_DEV_PCIE_REPLAY_COUNTER counter
DCGM_FI_DEV_PCIE_REPLAY_COUNTER{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_GPU_UTIL GPU utilization (in %).
# TYPE DCGM_FI_DEV_GPU_UTIL gauge
DCGM_FI_DEV_GPU_UTIL{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_MEM_COPY_UTIL Memory utilization (in %).
# TYPE DCGM_FI_DEV_MEM_COPY_UTIL gauge
DCGM_FI_DEV_MEM_COPY_UTIL{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_ENC_UTIL Encoder utilization (in %).
# TYPE DCGM_FI_DEV_ENC_UTIL gauge
DCGM_FI_DEV_ENC_UTIL{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_DEC_UTIL Decoder utilization (in %).
# TYPE DCGM_FI_DEV_DEC_UTIL gauge
DCGM_FI_DEV_DEC_UTIL{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_XID_ERRORS Value of the last XID error encountered.
# TYPE DCGM_FI_DEV_XID_ERRORS gauge
DCGM_FI_DEV_XID_ERRORS{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",err_code="0",err_msg="No Error",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_FB_FREE Framebuffer memory free (in MiB).
# TYPE DCGM_FI_DEV_FB_FREE gauge
DCGM_FI_DEV_FB_FREE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 20015
# HELP DCGM_FI_DEV_FB_USED Framebuffer memory used (in MiB).
# TYPE DCGM_FI_DEV_FB_USED gauge
DCGM_FI_DEV_FB_USED{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 1
# HELP DCGM_FI_DEV_FB_RESERVED Framebuffer memory reserved (in MiB).
# TYPE DCGM_FI_DEV_FB_RESERVED gauge
DCGM_FI_DEV_FB_RESERVED{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 457
# HELP DCGM_FI_DEV_UNCORRECTABLE_REMAPPED_ROWS Number of remapped rows for uncorrectable errors
# TYPE DCGM_FI_DEV_UNCORRECTABLE_REMAPPED_ROWS counter
DCGM_FI_DEV_UNCORRECTABLE_REMAPPED_ROWS{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_CORRECTABLE_REMAPPED_ROWS Number of remapped rows for correctable errors
# TYPE DCGM_FI_DEV_CORRECTABLE_REMAPPED_ROWS counter
DCGM_FI_DEV_CORRECTABLE_REMAPPED_ROWS{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_ROW_REMAP_FAILURE Whether remapping of rows has failed
# TYPE DCGM_FI_DEV_ROW_REMAP_FAILURE gauge
DCGM_FI_DEV_ROW_REMAP_FAILURE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_NVLINK_BANDWIDTH_TOTAL Total number of NVLink bandwidth counters for all lanes.
# TYPE DCGM_FI_DEV_NVLINK_BANDWIDTH_TOTAL counter
DCGM_FI_DEV_NVLINK_BANDWIDTH_TOTAL{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_DEV_VGPU_LICENSE_STATUS vGPU License status
# TYPE DCGM_FI_DEV_VGPU_LICENSE_STATUS gauge
DCGM_FI_DEV_VGPU_LICENSE_STATUS{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0
# HELP DCGM_FI_PROF_GR_ENGINE_ACTIVE Ratio of time the graphics engine is active.
# TYPE DCGM_FI_PROF_GR_ENGINE_ACTIVE gauge
DCGM_FI_PROF_GR_ENGINE_ACTIVE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0.004656
# HELP DCGM_FI_PROF_PIPE_TENSOR_ACTIVE Ratio of cycles the tensor (HMMA) pipe is active.
# TYPE DCGM_FI_PROF_PIPE_TENSOR_ACTIVE gauge
DCGM_FI_PROF_PIPE_TENSOR_ACTIVE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0.000000
# HELP DCGM_FI_PROF_DRAM_ACTIVE Ratio of cycles the device memory interface is active sending or receiving data.
# TYPE DCGM_FI_PROF_DRAM_ACTIVE gauge
DCGM_FI_PROF_DRAM_ACTIVE{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 0.003074
# HELP DCGM_FI_PROF_PCIE_TX_BYTES The rate of data transmitted over the PCIe bus - including both protocol headers and data payloads - in bytes per second.
# TYPE DCGM_FI_PROF_PCIE_TX_BYTES gauge
DCGM_FI_PROF_PCIE_TX_BYTES{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 78398480
# HELP DCGM_FI_PROF_PCIE_RX_BYTES The rate of data received over the PCIe bus - including both protocol headers and data payloads - in bytes per second.
# TYPE DCGM_FI_PROF_PCIE_RX_BYTES gauge
DCGM_FI_PROF_PCIE_RX_BYTES{gpu="0",UUID="GPU-a8394b51-c086-568a-9f1a-c1e1c934c44c",pci_bus_id="00000000:00:02.0",device="nvidia0",modelName="NVIDIA RTX 4000 Ada Generation",Hostname="lke529760-766837-0a634ab30000",DCGM_FI_DRIVER_VERSION="580.95.05",container="cuda",namespace="default",pod="cuda-75454ffb9f-8ljq7",pod_uid=""} 45987793
```

## AI Service Metrics

Prometheus is installable and usable on the LKE cluster

https://www.linode.com/docs/guides/deploy-prometheus-operator-with-grafana-on-lke/


## Secure Accelerator Access

Run kubernetes e2e DRA test suite.

Create two Pods, each is allocated an accelerator resource. Execute a command in one Pod to attempt to access the other Pod’s
accelerator, and should be denied.

**Step 1**: Make Kubernetes DRA e2e tests

```
mkdir k8s.io && \
git clone https://github.com/kubernetes/kubernetes
```
```
git checkout v1.34.1
```
```
make WHAT="ginkgo k8s.io/kubernetes/test/e2e/e2e.test"
```

**Step 2**: Run multi-container access test

```
KUBECONFIG=/Users/xxx/.kube/config _output/bin/ginkgo -v -focus='must map configs and devices to the right containers' ./test/e2e
```

```
  I1104 12:42:43.374995   58787 e2e.go:109] Starting e2e run "41597bc2-8046-4015-b863-8b98a1109aea" on Ginkgo node 1
Running Suite: Kubernetes e2e suite - /Users/srust/ws/github.com/linode/lke-ai-conformance/k8s.io/kubernetes/test/e2e
=====================================================================================================================
Random Seed: 1762278140 - will randomize all specs

Will run 1 of 7137 specs
------------------------------
[sig-node] [DRA] kubelet [Feature:DynamicResourceAllocation] must map configs and devices to the right containers [sig-node, DRA, Feature:DynamicResourceAllocation]
/Users/.../k8s.io/kubernetes/test/e2e/dra/dra.go:180
  STEP: Creating a kubernetes client @ 11/04/25 12:42:44.127
  STEP: Building a namespace api object, basename dra @ 11/04/25 12:42:44.128
  STEP: Waiting for a default service account to be provisioned in namespace @ 11/04/25 12:42:44.575
  STEP: Waiting for kube-root-ca.crt to be provisioned in namespace @ 11/04/25 12:42:44.649
  STEP: selecting nodes @ 11/04/25 12:42:44.726
  STEP: deploying driver dra-8441.k8s.io on nodes [lke530427-767898-51df6b5f0000] @ 11/04/25 12:42:44.776
  STEP: wait for plugin registration @ 11/04/25 12:42:47.302
  STEP: creating *v1.DeviceClass dra-8441-class @ 11/04/25 12:42:49.304
  STEP: creating *v1.DeviceClass dra-8441-class0 @ 11/04/25 12:42:49.351
  STEP: creating *v1.DeviceClass dra-8441-class1 @ 11/04/25 12:42:49.395
  STEP: creating *v1.DeviceClass dra-8441-class2 @ 11/04/25 12:42:49.44
  STEP: creating *v1.DeviceClass dra-8441-class3 @ 11/04/25 12:42:49.49
  STEP: creating *v1.DeviceClass dra-8441-class4 @ 11/04/25 12:42:49.529
  STEP: creating *v1.DeviceClass dra-8441-class5 @ 11/04/25 12:42:49.571
  STEP: creating *v1.ResourceClaim all @ 11/04/25 12:42:49.608
  STEP: creating *v1.ResourceClaim container0 @ 11/04/25 12:42:49.654
  STEP: creating *v1.ResourceClaim container1 @ 11/04/25 12:42:49.698
  STEP: creating *v1.Pod tester-1 @ 11/04/25 12:42:49.747
  STEP: delete pods and claims @ 11/04/25 12:42:56.398
  STEP: deleting *v1.Pod dra-8441/tester-1 @ 11/04/25 12:42:56.438
  STEP: deleting *v1.ResourceClaim dra-8441/all @ 11/04/25 12:43:00.667
  STEP: deleting *v1.ResourceClaim dra-8441/container0 @ 11/04/25 12:43:00.716
  STEP: deleting *v1.ResourceClaim dra-8441/container1 @ 11/04/25 12:43:00.759
  STEP: waiting for resources on lke530427-767898-51df6b5f0000 to be unprepared @ 11/04/25 12:43:00.807
  STEP: waiting for claims to be deallocated and deleted @ 11/04/25 12:43:00.808
  STEP: scaling down driver proxy pods for dra-8441.k8s.io @ 11/04/25 12:43:01.569
  STEP: Waiting for ResourceSlices of driver dra-8441.k8s.io to be removed... @ 11/04/25 12:43:02.025
  STEP: Destroying namespace "dra-8441" for this suite. @ 11/04/25 12:43:02.23

Ran 1 of 7137 Specs in 18.904 seconds
SUCCESS! -- 1 Passed | 0 Failed | 0 Pending | 7136 Skipped
PASS

Ginkgo ran 1 suite in 42.3242365s
Test Suite Passed
```

## Robust Controller 

### Install KubeRay Operator

Add the following helm repos and install the kuberay-operator

```shell
helm repo add kuberay https://ray-project.github.io/kuberay-helm/
helm repo update
helm install kuberay-operator kuberay/kuberay-operator --version 1.4.2
```

Verify that kuberay-operator pod is running and in Ready state

```shell
kubectl get pod
```
```shell
NAME                               READY   STATUS    RESTARTS   AGE
kuberay-operator-87c45b7f8-czg6d   1/1     Running   0          22s
```

### Install RayCluster

Install raycluster using the following helmchart

```shell
helm install raycluster kuberay/ray-cluster --version 1.4.2
```


Verify that raycluster resources are in Ready state

```shell
kubectl get rayclusters
```

```shell
NAME                 DESIRED WORKERS   AVAILABLE WORKERS   CPUS   MEMORY   GPUS   STATUS   AGE
raycluster-kuberay   1                 1                   2      3G       0      ready    4m18s
```

### Run KubeRay Job

Deploy a ray job for Modlin workload
```shell
kubectl apply -f https://raw.githubusercontent.com/ray-project/kuberay/master/ray-operator/config/samples/ray-job.modin.yaml
```

```
kubectl logs -l=job-name=rayjob-sample
```
```
2025-11-03 18:05:10,467	INFO worker.py:1554 -- Using address 10.2.2.187:6379 set in the environment variable RAY_ADDRESS
2025-11-03 18:05:10,467	INFO worker.py:1694 -- Connecting to existing Ray cluster at address: 10.2.2.187:6379...
2025-11-03 18:05:10,480	INFO worker.py:1879 -- Connected to Ray cluster. View the dashboard at 10.2.2.187:8265
Modin Engine: Ray
FutureWarning: DataFrame.applymap has been deprecated. Use DataFrame.map instead.
Time to compute isnull: 0.12875682900630636
Time to compute rounded_trip_distance: 0.4448743859975366
2025-11-03 18:05:46,335	SUCC cli.py:65 -- -----------------------------------
2025-11-03 18:05:46,335	SUCC cli.py:66 -- Job 'rayjob-sample-84rdk' succeeded
2025-11-03 18:05:46,335	SUCC cli.py:67 -- -----------------------------------

```
