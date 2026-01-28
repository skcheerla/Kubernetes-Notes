In Kubernetes, **API Versions** are used to categorize and version-control different types of resources. They are generally split into the **Core Group** (which has no group name in the string) and **Named Groups**.

Here is a categorized list of the most commonly used `apiVersion` values for major Kubernetes components as of 2026.

---

## 🏗️ 1. Core Resources

The core group (legacy group) is always just `v1`. You do not include a group name prefix for these.

| Kind | apiVersion | Description |
| --- | --- | --- |
| **Pod** | `v1` | Smallest unit; contains containers. |
| **Service** | `v1` | Exposes apps internally or externally. |
| **Namespace** | `v1` | Virtual partitions for the cluster. |
| **ConfigMap** | `v1` | Stores non-confidential configuration. |
| **Secret** | `v1` | Stores sensitive data (passwords/keys). |
| **Node** | `v1` | Physical or virtual machine information. |
| **PersistentVolume** | `v1` | Cluster-wide storage resource. |
| **PersistentVolumeClaim** | `v1` | Request for storage by a user/pod. |

---

## 📦 2. Workload Resources

These resources manage the lifecycle and scaling of your application containers.

| Kind | apiVersion | Description |
| --- | --- | --- |
| **Deployment** | `apps/v1` | Updates pods and manages scaling. |
| **StatefulSet** | `apps/v1` | Manages stateful apps (e.g., databases). |
| **DaemonSet** | `apps/v1` | Runs a pod on every node in the cluster. |
| **ReplicaSet** | `apps/v1` | Ensures a specific number of pod replicas. |
| **Job** | `batch/v1` | Runs a task once to completion. |
| **CronJob** | `batch/v1` | Runs jobs on a schedule (like Linux crontab). |

---

## 🌐 3. Networking & Discovery

These manage traffic routing and how services find each other.

| Kind | apiVersion | Description |
| --- | --- | --- |
| **Ingress** | `networking.k8s.io/v1` | HTTP/HTTPS external load balancing. |
| **NetworkPolicy** | `networking.k8s.io/v1` | Firewall rules between Pods. |
| **Gateway** | `gateway.networking.k8s.io/v1` | The newer standard for traffic routing. |
| **EndpointSlice** | `discovery.k8s.io/v1` | Efficient way to track Service endpoints. |

---

## 🔐 4. Configuration & Security

Resources used to manage cluster-wide settings and permissions.

| Kind | apiVersion | Description |
| --- | --- | --- |
| **Role / ClusterRole** | `rbac.authorization.k8s.io/v1` | Defines what actions are allowed. |
| **RoleBinding** | `rbac.authorization.k8s.io/v1` | Assigns a Role to a User/ServiceAccount. |
| **HorizontalPodAutoscaler** | `autoscaling/v2` | Automatically scales pods based on CPU/RAM. |
| **StorageClass** | `storage.k8s.io/v1` | Defines "profiles" for dynamic storage. |
| **CustomResourceDefinition** | `apiextensions.k8s.io/v1` | Allows you to create your own K8s objects. |

---

## 🛠️ Pro-Tip: How to find versions in your cluster

If you are ever unsure which version to use (especially if you have installed custom CRDs like Istio or ArgoCD), run these commands:

* **List all versions:** `kubectl api-versions`
* **List all resources and their versions:** `kubectl api-resources`
* **Get details on a specific kind:** `kubectl explain <kind>` (e.g., `kubectl explain deployment`)

> **Note on Stability:** > * **v1:** Stable and production-ready.
> * **v1beta1 / v2beta3:** Stable but features may still change slightly.
> * **v1alpha1:** Experimental; not recommended for production.
> 
> 

**Would you like me to generate a sample YAML manifest for a specific resource using its correct apiVersion?**
