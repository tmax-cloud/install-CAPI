
# Capi 설치 가이드

## 구성 요소 및 버전
* Cluster Api([github.com/kubernetes-sigs/cluster-api/latest](https://github.com/kubernetes-sigs/cluster-api/releases/latest))
* InfrastructureProvider-AWS ([https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/latest](https://github.com/kubernetes-sigs/cluster-api-provider-aws/releases/latest))

 ### **주의**
 _1. Capi는 public cloud와 원활한 통신을 위해 가급적 **최신 버전을 유지**해야 합니다. 최신 버전 확인은 위 링크를 통해 확인 바랍니다_
 <br>_2. 버전 관리 문제로 default yaml을 upload 하지 않았으나, install guide 단계 중 yaml download하는 과정이 있습니다_ 

## Prerequisites
* kubernetes version >= 1.16
* Cert Manager
    * [임시 설치 가이드](https://github.com/tmax-cloud/install-cert-manager-temp)

## 폐쇄망 설치 가이드
* 외부 네트워크 통신이 가능한 환경에서 0.preset-cn.sh를 이용하여 이미지 및 패키지 다운로드 후 1.2.install-capi-cn.sh를 이용하여 폐쇄망에 CAPI 환경 구성
* 외부 네트워크 환경 스크립트 실행순서
    ```bash
    $ cd manifest
    $ chmod +x *.sh
    $ source version.conf
    $ bash 0.preset-cn.sh
    ```

* 폐쇄망 설치 스크립트 실행순서
    * Capi 설치
    ```bash
    $ cd manifest
    $ source version.conf
    $ export REGISTRY={registryIP:PORT}
    $ bash 1.2.install-capi-cn.sh
    ```
    
    * Provider 설치
        1. [AWS Provider]
            * AWS Config 값을 환경변수로 등록후 스크립트 실행
            ```bash
            $ export AWS_REGION=your_region
            $ export AWS_ACCESS_KEY_ID=your_access_key_id
            $ export AWS_SECRET_ACCESS_KEY=your_secret_access_key_id
            $ bash 2.1.2.install-aws-cn.sh
            ```

    * Cluster provisioning
        1. [AWS Provider]
            * AWS Config 값을 환경변수로 등록후 스크립트 실행
            ```bash
            $ export AWS_REGION=your_region
            $ export CLUSTER_NAME=your_cluster_name
            $ export AWS_CONTROL_PLANE_MACHINE_TYPE=t3.large
            $ export AWS_NODE_MACHINE_TYPE=t3.large
            $ export AWS_SSH_KEY_NAME=your_aws_ssh_key_name
            $ export KUBERNETES_VERSION=kubernetes_version
            $ export CONTROL_PLANE_MACHINE_COUNT=control_plane_machine_count
            $ export WORKER_MACHINE_COUNT=worker_machine_count
            $ bash 3.1.2.install-aws-cn.sh
            ```
## Install Steps(Open Network)
* Capi 설치에 필요한 리소스(yaml, binary)다운로드 및 설치
    ```bash
    $ cd manifest
    $ chmod +x *.sh
    $ source version.conf
    $ bash 1.1.install-capi-on.sh
    ```

* Provider 설치
    1. [AWS Provider]
        * AWS Config 값을 환경변수로 등록후 스크립트 실행
            ```bash
            $ export AWS_REGION=your_region
            $ export AWS_ACCESS_KEY_ID=your_access_key_id
            $ export AWS_SECRET_ACCESS_KEY=your_secret_access_key_id
            $ export CLUSTER_NAME=your_cluster_name
            $ export AWS_CONTROL_PLANE_MACHINE_TYPE=t3.large
            $ export AWS_NODE_MACHINE_TYPE=t3.large
            $ export AWS_SSH_KEY_NAME=your_aws_ssh_key_name
            $ export KUBERNETES_VERSION=kubernetes_version
            $ export CONTROL_PLANE_MACHINE_COUNT=control_plane_machine_count
            $ export WORKER_MACHINE_COUNT=worker_machine_count
            $ bash 2.1.1.install-aws-on.sh
            ```

* Cluster provisioning
    1. [AWS Provider]
        * AWS Config 값을 환경변수로 등록후 스크립트 실행
        ```bash
        $ export AWS_REGION=your_region
        $ export CLUSTER_NAME=your_cluster_name
        $ export AWS_CONTROL_PLANE_MACHINE_TYPE=t3.large
        $ export AWS_NODE_MACHINE_TYPE=t3.large
        $ export AWS_SSH_KEY_NAME=your_aws_ssh_key_name
        $ export KUBERNETES_VERSION=kubernetes_version
        $ export CONTROL_PLANE_MACHINE_COUNT=control_plane_machine_count
        $ export WORKER_MACHINE_COUNT=worker_machine_count
        $ bash 3.1.1.install-aws-on.sh
        ```
## Uninstall Steps
* Cluster 삭제
    1. [AWS Provider]
    * 실행 순서
        ```bash
        $ cd manifest
        $ source version.conf
        $ bash 3.1.3.delete-cluster-aws.sh

* Provider 삭제
    1. [AWS Provider]
    * 실행 순서
        ```bash
        $ cd manifest
        $ source version.conf
        $ bash 2.1.3.delete-aws.sh
        ```

* CAPI와 Provider들의 CRD 제거 및 바이너리, yaml등의 리소스 삭제
    ```bash
    $ source version.conf
    $ bash 99.delete.sh
    ```