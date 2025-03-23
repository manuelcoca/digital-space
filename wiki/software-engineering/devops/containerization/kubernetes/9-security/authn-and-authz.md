## Authentication (AuthN)

Securing access to the Kubernetes Cluster.

Kubernetes does not manage user accounts natively. It relies on an external source like a file with user details or certificates or a third party identity service like LDAP to manage these users.

On each request to the kube-apiserver, the server authenticates the request. Authentication mechanism to authenticate against the kube-apiserver:

-   Static password files
-   Static token files
-   Certificates
-   3rd Party Identity Services

### Static password files

#### **Setup basic authentication on Kubernetes with static password files (Deprecated in 1.19)**

> Note: This is not recommended in a production environment. This is only for learning purposes. Also note that this approach is deprecated in Kubernetes version 1.19 and is no longer available in later releases

Follow the below instructions to configure basic authentication in a kubeadm setup.

Create a file with user details locally at `/tmp/users/user-details.csv`

```
<password>,<username>,<user-id>,<user-group>
<password>,<username>,<user-id>,<user-group>
<password>,<username>,<user-id>,<user-group>
<password>,<username>,<user-id>,<user-group>
```

```
> # User File Contentspassword123,user1,u0001password123,user2,u0002password123,user3,u0003password123,user4,u0004password123,user5,u0005
```

Edit the kube-apiserver static pod configured by kubeadm to pass in the user details. The file is located at `/etc/kubernetes/manifests/kube-apiserver.yaml`

```
apiVersion: v1kind: Podmetadata:  name: kube-apiserver  namespace: kube-systemspec:  containers:  - command:    - kube-apiserver      <content-hidden>    image: k8s.gcr.io/kube-apiserver-amd64:v1.11.3    name: kube-apiserver    volumeMounts:    - mountPath: /tmp/users      name: usr-details      readOnly: true  volumes:  - hostPath:      path: /tmp/users      type: DirectoryOrCreate    name: usr-details
```

Modify the kube-apiserver startup options to include the basic-auth file

```
apiVersion: v1kind: Podmetadata:  creationTimestamp: null  name: kube-apiserver  namespace: kube-systemspec:  containers:  - command:    - kube-apiserver    - --authorization-mode=Node,RBAC      <content-hidden>    - --basic-auth-file=/tmp/users/user-details.csv
```

Create the necessary roles and role bindings for these users:

```
---kind: RoleapiVersion: rbac.authorization.k8s.io/v1metadata:  namespace: default  name: pod-readerrules:- apiGroups: [""] # "" indicates the core API group  resources: ["pods"]  verbs: ["get", "watch", "list"] ---# This role binding allows "jane" to read pods in the "default" namespace.kind: RoleBindingapiVersion: rbac.authorization.k8s.io/v1metadata:  name: read-pods  namespace: defaultsubjects:- kind: User  name: user1 # Name is case sensitive  apiGroup: rbac.authorization.k8s.ioroleRef:  kind: Role #this must be Role or ClusterRole  name: pod-reader # this must match the name of the Role or ClusterRole you wish to bind to  apiGroup: rbac.authorization.k8s.io
```

Once created, you may authenticate into the kube-api server using the users credentials

`curl -v -k https://localhost:6443/api/v1/pods -u "user1:password123"`

### Using Certificates

Certificates can be configured in the [KubeConfig](kubeconfig.md) file.

## Authorization (AuthZ)

Authorization helps to ristrict access within the cluster. Example

-   Only Admin users are allowed to delete Deployments
-   Developers only are allowed to view resources
-   etc.

### Authorization Mechanisms

1. Node
2. ABAC
3. [RBAC](rbac.md)
4. Webhook
