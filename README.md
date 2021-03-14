###### Descomplicando o Kubernetes

### Instalando o minikube

    - Faça do donwload do minikube
    - minikube start (se tiver o docker instalado, ele faz a criação dos confs lá)

#### Instalando o Docker com Kubernetes

    - Precisa ajustar algumas coisas, dentro do diretório /etc/docker crie o arquivos daemon.js
    ```
    {
        "exec-opts": ["native.cgroupdriver=systemd"],
        "log-driver": "json-file",
        "log-opts": { 
                "max-size": "100m"
        },
        "storage-driver": "overlay2"
    } 
    ```
    - Precisa criar este folder /etc/systemd/system/docker.service.d
    - systectl daemon-reload
    - systemctl restart docker.services

## Instalando o Kubernetes

    - Url https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-using-native-package-management

    - Depois da leitura faça e instalação dos repos, instale:
        - apt-get update
        - apt-get install -y kubeadm kubelet kubectl

    - Depois vamos iniciar o cluster no master
        > kubeadm config images pull 
        > kubeadm init
        > Saída do comando acima 
        ```
        Your Kubernetes control-plane has initialized successfully!

        To start using your cluster, you need to run the following as a regular user:

        mkdir -p $HOME/.kube
        sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
        sudo chown $(id -u):$(id -g) $HOME/.kube/config

        Alternatively, if you are the root user, you can run:

        export KUBECONFIG=/etc/kubernetes/admin.conf

        You should now deploy a pod network to the cluster.
        Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
        https://kubernetes.io/docs/concepts/cluster-administration/addons/

        Then you can join any number of worker nodes by running the following on each as root:

        kubeadm join 172.31.33.85:6443 --token mwvwyd.vdaw6kfwpn6rnayp \
            --discovery-token-ca-cert-hash sha256:9cb2d9b1440d9d583069443de42b92f2d0e44d4749cb43a54634299f0db7d781
        ```    
       - Ativando alguns modulos do Kernel
        - modprobe br_netfilter ip_vs_rr ip_vs_wrr ips_vs_sh nf_conntrack_ipv4 ip_vs
       - Instalando o wavenet POD
        > kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

       - Depois vamos fazer o join dos works no master
         > kubeadm join 172.31.33.85:6443 --token mwvwyd.vdaw6kfwpn6rnayp \
            --discovery-token-ca-cert-hash sha256:9cb2d9b1440d9d583069443de42b92f2d0e44d4749cb43a54634299f0db7d781
       - Recuperando o token do cluster 
        > kubeadm token create --print-join-command    


### Comandos básicos

    > kubectl create namespace course
    > kubectl config set-context --current --namespace=course # Mudando o namespace padrão
    > kubectl -n course run nginx --image=nginx # de forma simples
    > kubectl -n course run nginx --image=nginx --dry-run=client -o yaml > teste.yaml # Criando e redirecionando para um arquivo 
    > kubectl -n course apply  -f teste.yaml # pega a saída do comando acima e cria a pod
    > kubectl get pods,services # List com sequêncial de serviços

## Context do usuário 

    > kubectl config get-contexts
    > kubectl config use-context kubernetes-admin@kubernetes --kubeconfig=${HOME}/.kube/config

## Manpage do kubernetes 

> kubectl explain services # Explica o que seria o sub comando services



