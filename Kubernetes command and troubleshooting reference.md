Here's a comprehensive breakdown of Kubernetes (`kubectl`) commands and troubleshooting scenarios, organized for easy understanding with practical examples:

---

## **✅ Core Management Commands**

### **1️⃣ `kubectl get` – List Resources**
- **Purpose**: View resources in your cluster.
  ```sh
  kubectl get pods           # List all pods
  kubectl get deployments    # List deployments
  kubectl get services       # List services
  kubectl get all            # List most resources in the current namespace
  kubectl get nodes          # List all nodes in the cluster
  ```
  **Example**:  
  ```sh
  kubectl get pods -n default  # Lists pods in the `default` namespace
  ```

### **2️⃣ `kubectl describe` – Detailed Resource Info**
- **Purpose**: Get detailed information about a resource (events, configs, errors).
  ```sh
  kubectl describe pod my-pod      # Details of a pod
  kubectl describe node my-node   # Details of a node
  ```
  **Example**:  
  ```sh
  kubectl describe pod nginx-abc123  # Checks why a pod is failing
  ```

### **3️⃣ `kubectl create` – Create Resources**
- **Purpose**: Create resources from a YAML/JSON file.
  ```sh
  kubectl create -f my-deployment.yaml  # Creates a deployment
  ```
  **Example**:  
  ```sh
  kubectl create namespace dev  # Creates a namespace named `dev`
  ```

### **4️⃣ `kubectl apply` – Apply Configurations**
- **Purpose**: Apply or update resources (idempotent).
  ```sh
  kubectl apply -f my-config.yaml  # Applies changes
  ```
  **Example**:  
  ```sh
  kubectl apply -f https://example.com/nginx-deployment.yaml
  ```

### **5️⃣ `kubectl delete` – Remove Resources**
- **Purpose**: Delete resources.
  ```sh
  kubectl delete pod my-pod       # Deletes a pod
  kubectl delete svc my-service   # Deletes a service
  ```
  **Example**:  
  ```sh
  kubectl delete deployment nginx --force  # Forcefully deletes a deployment
  ```

---

## **✅ Debugging & Troubleshooting**

### **6️⃣ `kubectl logs` – View Pod Logs**
- **Purpose**: Check logs for debugging.
  ```sh
  kubectl logs my-pod                     # Pod logs
  kubectl logs my-pod -c my-container     # Logs from a specific container
  kubectl logs --tail=100 my-pod          # Last 100 lines
  kubectl logs -f my-pod                  # Stream logs in real-time
  ```
  **Example**:  
  ```sh
  kubectl logs nginx-pod --since=5m  # Logs from the last 5 minutes
  ```

### **7️⃣ `kubectl exec` – Execute Commands in a Pod**
- **Purpose**: Run commands inside a pod (like SSH).
  ```sh
  kubectl exec -it my-pod -- bash   # Interactive shell
  kubectl exec my-pod -- ls /app    # Run a single command
  ```
  **Example**:  
  ```sh
  kubectl exec -it redis-pod -- redis-cli  # Access Redis CLI
  ```

### **8️⃣ `kubectl port-forward` – Access Pods Locally**
- **Purpose**: Forward a local port to a pod.
  ```sh
  kubectl port-forward my-pod 8080:80  # Forward local 8080 → pod’s 80
  ```
  **Example**:  
  ```sh
  kubectl port-forward nginx-pod 8080:80  # Access nginx at `localhost:8080`
  ```

### **9️⃣ `kubectl top` – Monitor Resource Usage**
- **Purpose**: Check CPU/memory usage.
  ```sh
  kubectl top pods    # Resource usage of pods
  kubectl top nodes   # Resource usage of nodes
  ```

### **🔟 `kubectl explain` – Documentation for Resources**
- **Purpose**: Understand Kubernetes resource structures.
  ```sh
  kubectl explain pod          # Docs for Pods
  kubectl explain pod.spec     # Specific field docs
  ```

---

## **✅ Managing Workloads**

### **1️⃣1️⃣ `kubectl rollout` – Manage Deployments**
- **Purpose**: Check/undo deployments.
  ```sh
  kubectl rollout status deployment/my-deployment  # Check status
  kubectl rollout undo deployment/my-deployment   # Rollback to previous version
  ```

### **1️⃣2️⃣ `kubectl scale` – Scale Deployments**
- **Purpose**: Increase/decrease replicas.
  ```sh
  kubectl scale deployment/my-deployment --replicas=5  # Scale to 5 pods
  ```

### **1️⃣3️⃣ `kubectl edit` – Modify Resources Live**
- **Purpose**: Edit a resource directly.
  ```sh
  kubectl edit deployment my-deployment  # Opens YAML in default editor
  ```

---

## **🔧 Common Troubleshooting Scenarios**

| **Issue** | **Debugging Command** | **Solution** |
|-----------|----------------------|--------------|
| **Pod CrashLoopBackOff** | `kubectl logs <pod>` | Check container logs for errors |
| **ImagePullBackOff** | `kubectl describe pod <pod>` | Verify image name & registry auth |
| **Pending Pod** | `kubectl describe pod <pod>` | Check resource limits/node availability |
| **Service Not Accessible** | `kubectl get endpoints` | Verify service selector matches pod labels |
| **DNS Issues** | `kubectl exec -it <pod> -- nslookup google.com` | Check CoreDNS pod logs |
| **RBAC Permissions** | `kubectl auth can-i create pods` | Adjust `Role`/`ClusterRole` bindings |
| **Node Not Ready** | `kubectl describe node <node>` | Check kubelet logs on the node |

---

## **💡 Pro Tips**
1. **Use `-n` for Namespace**:  
   ```sh
   kubectl get pods -n kube-system  # View system pods
   ```
2. **Shortcuts**:  
   - `po` = pods, `svc` = services, `deploy` = deployments.
   ```sh
   kubectl get po  # Equivalent to `kubectl get pods`
   ```
3. **JSON Output**:  
   ```sh
   kubectl get pods -o json  # Machine-readable output
   ```

---

### **Summary**
- **Core Commands**: `get`, `describe`, `create`, `apply`, `delete`.
- **Debugging**: `logs`, `exec`, `port-forward`, `describe`.
- **Workloads**: `rollout`, `scale`, `edit`.
- **Troubleshooting**: Check logs, events, and resource allocations.

Mastering these commands will make you a **Kubernetes debugging ninja**! 🚀  

Let me know if you'd like a deeper dive into any topic!