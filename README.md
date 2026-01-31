
### Prerequisite

local env
```bash
➜  sw_vers
ProductName:	macOS
ProductVersion:	12.7.6
```


```bash
# download kubebuilder and install locally.
curl -L -o kubebuilder "https://go.kubebuilder.io/dl/latest/$(go env GOOS)/$(go env GOARCH)"
chmod +x kubebuilder && sudo mv kubebuilder /usr/local/bin/

# check version
➜  kubebuilder version
KubeBuilder:          v4.11.1
Kubernetes:           1.35.0
Git Commit:           4fbe8f6c63c6eb9a2a5cc440acbedad6b60d3225
Build Date:           2026-01-27T16:16:09Z
Go OS/Arch:           darwin/amd64
```

reference:
  -  https://book.kubebuilder.io/quick-start.html
  -  https://github.com/daniel-hutao/Advanced-Kubernetes-Operator

### Set up Application Operator Demo

```bash
cd operator-practice
mkdir application-operator


# init 
cd application-operator
kubebuilder init --domain jack.cn \
--repo jack.cn/application-operator \
--owner Jack

# add API
cd application-operator
kubebuilder create api \
--group apps --version v1 --kind Application
# enter "y" for twice

# edit, code diff
ls -l api/v1/application_types.go

```

```diff
+	corev1 "k8s.io/api/core/v1"
)

// EDIT THIS FILE!  THIS IS SCAFFOLDING FOR YOU TO OWN!
// NOTE: json tags are required.  Any new fields you add must have json tags for the fields to be serialized.

// ApplicationSpec defines the desired state of Application
type ApplicationSpec struct {
...
	// More info: https://book.kubebuilder.io/reference/markers/crd-validation.html

	// foo is an example field of Application. Edit application_types.go to remove/update
	// +optional
-	// Foo *string `json:"foo,omitempty"`
+	Replicas int32                   `json:"replicas,omitempty"`  
+	Template corev1.PodTemplateSpec  `json:"template,omitempty"`  
}
```


```bash
make manifests
make install

# check crd from local k8s cluster
➜  operator-practice git:(main) ✗ kubectl get crd
NAME                        CREATED AT
applications.apps.jack.cn   2026-01-31T08:24:51Z
```

modify `config/samples/apps_v1_application.yaml `

```bash
kubectl apply -f config/samples/apps_v1_application.yaml 

➜  operator-practice git:(main) ✗ kubectl get application
NAME                 AGE
application-sample   18s
```

modify `internal/controller/application_controller.go` to implement Reconcile()

then run `make run` to run a controller from your host