<h1 align="center">
  <br>
  Ixia-c-helm
  <br>
</h1>

<h4 align="center">
  Helmfile based deployment solution for <a href="https://hub.docker.com/r/ixiacom/ixia-c-controller" target="_blank"> Ixia-c controller</a> and <a href="https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine" target="_blank"> Ixia-c traffic engine </a> in kubernetes
</h4>

<p align="center">
  <a href="https://github.com/open-traffic-generator/ixia-c-helm/releases/tag/v0.0.2"><img alt="Release v0.0.2" src="https://img.shields.io/badge/release-v0.0.2-brightgreen"></a>
  <a href="https://github.com/open-traffic-generator/ixia-c/releases/tag/v0.0.1-2610"><img alt="ixia-c v0.0.1-2610" src="https://img.shields.io/badge/ixia--c-v0.0.1--2610-brightgreen"></a>
</p


Ixia-c-helm is tool to simplify the deployment of topologies for [Ixia-c controller](https://hub.docker.com/r/ixiacom/ixia-c-controller) and [Ixia-c traffic engine](https://hub.docker.com/r/ixiacom/ixia-c-traffic-engine).

## Quick Start
- Please make sure your setup is ready with all the dependecies installed. Please follow [Prerequisites](#prerequisites) for this. 

- Add helm repo 
  ```sh
  helm repo add ixia-c-helm https://raw.githubusercontent.com/open-traffic-generator/ixia-c-helm/main/
  ```

- Check all the local helm chart repo list 
  ```sh
  helm repo list 
  ```

- Deploy back-2-back topology using `helmfile`
    - Get sample files
        ```sh
        git clone https://github.com/open-traffic-generator/ixia-c-helm.git && cd ixia-c-helm/samples/back-2-back
        ``` 
    - Deploy/install topologies
        ```sh
        helmfile sync
        ```
    - Delete/uninstall topologies
        ```sh
        helmfile delete
        ```
    

## Getting Started
- Please make sure your setup is ready with all the dependecies installed. Please follow [Prerequisites](#prerequisites) for this. 
- Add helm repo 
  ```sh
  helm repo add <local-repo-name> https://raw.githubusercontent.com/open-traffic-generator/ixia-c-helm/main/
  ```
  - example
    ```sh
    helm repo add ixia-c-helm https://raw.githubusercontent.com/open-traffic-generator/ixia-c-helm/main/
    ```

- Check all the local helm chart repo list 
  ```sh
  helm repo list 
  ```
  
- Update helm repo to the latest version available
  ```sh
  helm repo update <local-repo-name>
  ```
  - example
    ```sh
    helm repo update ixia-c-helm 
    ```

- Search helm charts 
    ```sh 
    helm search repo <local-repo-name>
    ```
    - example
        ```sh
        helm search repo ixia-c-helm 
        ```

- Check all helm charts details with versions
    ```sh 
    helm search repo <local-repo-name> -l
    ```
    - example
        ```sh
        helm search repo ixia-c-helm -l
        ```

- Check all helm charts details for a specific version
    ```sh 
    helm search repo <local-repo-name> --version=<chart-version>
    ```
    - example -
        ```sh
        helm search repo ixia-c-helm --version=0.0.1
        ```

- Check helm envioronment variables
    ```sh 
    helm env 
    ```

- Check all contents(in chart.yaml & values.yaml) for a specific helmchart
    ```sh 
    helm show all <chart-name>
    ```
    - example
        ```sh
        helm show all ixia-c-helm/ixia-c-controller 
        ```

- Check contents in Chart.yaml for a specific helmchart
    ```sh 
    helm show chart <chart-name>
    ```
    - example
        ```sh
        helm show chart ixia-c-helm/ixia-c-controller 
        ```

- Check contents in values.yaml for a specific helmchart
    ```sh 
    helm show values <chart-name>
    ```
    - example
        ```sh
        helm show values ixia-c-helm/ixia-c-controller 
        ```

## Deploy Topology
#### Deploy using `helm istall`
- deploy with default values
    ```sh
    helm install <release-name> <chart-name> 
    ```
    - example 
        ```sh
        helm install ixia-c-controller ixia-c-helm/ixia-c-controller
        ```
- delete releases using `helm uninstall`
    ```sh
    helm uninstall <release-name> 
    ```
    - example 
        ```sh
        helm uninstall ixia-c-controller 
        ```

#### Deploy using `helmfile`
- Get sample files
    - Clone this repository
        ```sh
        git clone https://github.com/open-traffic-generator/ixia-c-helm.git && cd ixia-c-helm/samples
        ``` 
- Deploy/install topologies
    - go to specific topology folder and execute the below command
        ```sh
        helmfile sync
        ```
- Delete/uninstall topologies
    - go to specific topology folder and execute the below command
        ```sh
        helmfile delete
        ```

- **Customize** your helmfile based on your requirement 
    - Modify the chart name according to the local name in helmfile.yaml 
        ```sh 
        releases:
            - chart: ixia-c-helm/ixia-c-controller 
        ```
    - Modify interface and ports based on the requirements
        ```sh 
        set:
        - name: name
            value: ixia-c-traffic-engine-svc-1
        - name: egressDevice
            value: veth1
        - name: service.port
            value: 5555
        ```
    - If your want to use a different chart version add accordingly in helmfile.yaml (If you don't mention anything it will use the lastest chart version)
        ```sh 
        releases:
            - chart: ixia-c-helm/ixia-c-controller 
                version: 0.0.1
        ```

## Prerequisites

- x86-64 Ubuntu 20.04 Server
- At least 4 CPU cores, 8GB RAM and 128GB HDD
- Please make sure you have healthy k8s cluster running.
- Ensure existing network interfaces are `Up` and have `Promiscuous` mode enabled.

   ```sh
   # check interface details
   ip addr
   # configure as required
   ip link set eth1 up
   ip link set eth1 promisc on
   ```
- (Optional) To deploy `ixia-c-traffic-engine` against veth interface pairs, you need to create them as follows:

   ```sh
   # create veth pair veth1 and veth2
   ip link add veth1 type veth peer name veth2
   ip link set veth1 up
   ip link set veth2 up
- Please make sure calico service is deployed.
    ```sh
    curl https://docs.projectcalico.org/manifests/calico.yaml -O
    kubectl apply -f calico.yaml
    ```
- Please make sure helm is installed.
    ```sh
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh
    ```
- Please make sure helmfile is installed.
    - Install using wget 
        ```sh
        wget -O helmfile_linux_amd64 https://github.com/roboll/helmfile/releases/download/v0.135.0/helmfile_linux_amd64
        chmod +x helmfile_linux_amd64
        mv helmfile_linux_amd64 ~/.local/bin/helmfile
        ```
    - Install using brew
        ```sh
        brew install helmfile
        ```
        - Install brew
            ```sh
            sudo apt update
            sudo apt-get install build-essential
            sudo apt install git -y
            /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
            eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
            brew doctor
            brew install gcc
            ```
    - Reference
        - https://github.com/roboll/helmfile
        - https://www.codegrepper.com/code-examples/shell/helmfile+install+ubuntu

        