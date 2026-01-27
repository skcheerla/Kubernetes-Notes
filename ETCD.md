Kubernetes Components :

Master Node ( Manage, Plan, Schedule and Monitor Nodes)

Kube-apiserver

etcd Cluster

Controller Manager

Kube-scheduler


Worker Nodes ( Host applications As containers)

Kubelet

KubeProxy

container Run Time Engine ( Containerd , rkt)

==================================================

Docker

containerd - CNCF Member


CRI - Container RunTime Interface


==============================================

ETCD :

etcd is an open-source, distributed, strongly consistent key-value store

Role in K8s: It stores the entire state of the cluster (Nodes, Pods, ConfigMaps, Secrets, Accounts, roles , Bindings and others ...etc.).

Interaction: Only the kube-apiserver interacts directly with etcd.

Consistency: It uses the Raft Consensus Algorithm to ensure that all nodes in an etcd cluster agree on the data

2379: For Client Communication. Used by the kube-apiserver to talk to etcd.

2380: For Server-to-Server (Peer) Communication. Used by etcd nodes to talk to each other to maintain the cluster and elect a leader

Static Pod Manifest: /etc/kubernetes/manifests/etcd.yaml (if installed via kubeadm)

Data Directory: /var/lib/etcd (where the actual database files and Write-Ahead Logs (WAL) are stored).

Certs: /etc/kubernetes/pki/etcd/ (contains the TLS certificates for secure communication)

High Availability (Quorum): etcd should always have an odd number of nodes (3, 5, or 7). This is to avoid a "Split Brain" scenario. The formula for Quorum is $(n/2) + 1$

The "Watch" Mechanism: The API server "watches" specific keys in etcd. When a change occurs (e.g., a new Pod is created), etcd notifies the API server, which then triggers the Controller or Scheduler


Backup and Restore: You use the etcdctl utility.

Snapshot: etcdctl snapshot save snapshot.db

Restore: etcdctl snapshot restore snapshot.db


Performance: etcd is highly sensitive to Disk I/O latency. It is a best practice to run etcd on SSDs to avoid leader election timeouts.

. Sample Interview Questions

"What happens if etcd goes down?"

Answer: The cluster keeps running (existing Pods stay up), but you cannot make any changes. You can't create new Pods, delete existing ones, or update any configurations because the "source of truth" is unavailable.

"Why use an odd number of nodes?"

Answer: To ensure a clear majority for the Raft algorithm. An even number of nodes doesn't provide more fault tolerance but makes reaching a majority harder during network partitions.


ETCD Server :

ps -ef | grep -I etcd

ETCD Client :

etcdctl put key1 value1

etcdctl get key1 ( To retrieve the data from DB)

etcdctl ( will give all available options in this command)

ETCd Versions:

etcdctl version

Current Stable Versions (2026)

Kubernetes typically supports a specific range of etcd versions. For the most recent Kubernetes releases (v1.33–v1.35):

etcd v3.6.x (Latest): The current major focus. It introduced v3store as the sole source of truth and removed the legacy v2store flags.

etcd v3.5.x (Legacy Stable): Still widely used in many production environments and cloud-managed services (like older EKS/GKE clusters).


ETCD Manual Installation

download Binary and Setup service File

etcd.service

ExecStart : /usr/local/bin/etcd

advertise Client URL :  https://INTERNALIP:2379


setup via Kubeadm

kubectl get pods -n kube-system

kubectl exec etcd-master -n kube-system etcdctl get / --prefix -keys-only

ETCD stores data like below

/registry/


sub directories

minions

pods

replicasets

deployments

roles

secrets

====================

ETCD in HA Environment contains multiple ETCD servers

etcd.service ( config File)

--intila-cluster - controller0:https://controller0-Ip:2380 , controller1:https://controller1-Ip:2380 


(Optional) Additional information about ETCDCTL Utility

ETCDCTL is the CLI tool used to interact with ETCD.

ETCDCTL can interact with ETCD Server using 2 API versions - Version 2 and Version 3.  By default its set to use Version 2. Each version has different sets of commands.

To set the right version of API set the environment variable ETCDCTL_API command

export ETCDCTL_API=3


Whereas the commands are different in version 3

etcdctl snapshot save 
etcdctl endpoint health
etcdctl get
etcdctl put

============================================================================





