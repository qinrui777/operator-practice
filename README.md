
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

### Set up Application Operator Demo

```bash
cd operator-practice
mkdir application-operator

kubebuilder init --domain jack.cn \
--repo jack.cn/application-operator \
--owner Jack
```
