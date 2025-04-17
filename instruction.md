# INSTRUCTION.md

## Testing the Application

This document provides instructions to test the ToDo application through various methods:

---

### 1. Testing Application via ClusterIP Service DNS (From a BusyBox Container)

1. **Deploy the BusyBox Pod**:
   Run the following command to create a BusyBox pod for testing:
   ```bash
   kubectl run busybox --image=busybox --restart=Never --command -- sleep 3600
   ```

2. **Exec into the BusyBox Pod**:
   Once the pod is running, access it using:
   ```bash
   kubectl exec -it busybox -- sh
   ```

3. **Query the ClusterIP Service DNS**:
   Run the command below, replacing `<service-name>` and `<namespace>` with the respective service's name and namespace:
   ```bash
   wget -qO- <service-name>.<namespace>.svc.cluster.local:<port>
   ```
   Example:
   ```bash
   wget -qO- todo-app.default.svc.cluster.local:8080
   ```

---

### 2. Testing ToDo Application Using Service Port-Forward Command

1. **Find the Service**:
   Identify the service associated with the ToDo application:
   ```bash
   kubectl get svc
   ```

2. **Port-Forward the Service**:
   Use the following command to forward a local port to the service's port:
   ```bash
   kubectl port-forward svc/<service-name> <local-port>:<service-port>
   ```
   Example:
   ```bash
   kubectl port-forward svc/todo-app 8080:8080
   ```

3. **Access the Application**:
   Open your browser or use a tool like `curl` to access the application at:
   ```bash
   http://localhost:<local-port>
   ```
   Example:
   ```bash
   http://localhost:8080
   ```

---

### 3. Accessing the ToDo Application via NodePort Service

1. **Find the NodePort Details**:
   Identify the NodePort service and its port by running:
   ```bash
   kubectl get svc
   ```
   Output example:
   ```
   NAME         TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
   todo-app     NodePort       10.96.109.230   <none>        8080:30007/TCP   14m
   ```

   In this example, `30007` is the NodePort.

2. **Access the Application via Node Port**:
   Retrieve the IP address of a Kubernetes node (master or worker):
   ```bash
   kubectl get nodes -o wide
   ```
   Use the node's IP and NodePort to access the application, e.g.:
   ```bash
   http://<node-ip>:<node-port>
   ```
   Example:
   ```bash
   http://192.168.1.100:30007
   ```

---

Follow these steps to verify the functionality of the ToDo application and perform testing as required.