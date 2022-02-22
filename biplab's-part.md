<h1 align="center">
  <br>
  Ixia-C-helm
  <br>
</h1>

<h4 align="center">
  Helm based deployment solution for Ixia-C traffic generator in kubernetes
</h4>

<p align="center">
  <a href="https://github.com/open-traffic-generator/ixia-c-helm/releases/tag/v0.0.2"><img alt="Release v0.0.2" src="https://img.shields.io/badge/release-v0.0.2-brightgreen"></a>
  <a href="https://github.com/open-traffic-generator/ixia-c/releases/tag/v0.0.1-2610"><img alt="ixia-c v0.0.1-2610" src="https://img.shields.io/badge/ixia--c-0.0.1--2610-brightgreen"></a>
</p


### Prerequisites

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


### Get Started

- Clone this repository
  ```sh
  git clone https://github.com/open-traffic-generator/ixia-c-helm.git && cd ixia-c-helm
  ```
- Add helm repo
  ```sh
  helm repo add ixia-c <location_of_helm_chart>
  ```
  - example

    ```sh
    helm repo add helm <ixia-c-helm release link>
    ```
- check all the local helm chart repo list 

  ```sh
  helm repo list -a
  ```
- search helm charts 

```sh 
    helm search repo <local_repo_name>
```
- check helm envioronment variables

```sh 
   helm env 
```
- check the releases of helm chart based on the namespace 

```sh 
   helm list -a 
```
- check all contents(in chart.yaml & values.yaml) for a specific helmchart

```sh 
    helm show all helm/my-chart
```
- check contents in Chart.yaml for a specific helmchart

```sh 
    helm show chart helm/my-chart
```
- check contents in values.yaml for a specific helmchart

```sh 
    helm show values helm/my-chart
```
- check the details about the release of helm chart 

```sh 
    helm status <release_name>
```

- Create Topology
   - samples available 

    ```sh
    sudo ./containerlab deploy --topo ixia-c-one-ceos.clab.yaml
    ```

        