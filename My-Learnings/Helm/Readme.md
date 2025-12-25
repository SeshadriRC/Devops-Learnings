- [Install helm](#Install-helm)
- [Helm-Chart-Structure](#helm-chart-structure)
- [values-file](#values-file)

# Install helm

- Follow this [doc](https://helm.sh/docs/intro/install#from-apt-debianubuntu)

  
# Helm Chart Structure

A Helm chart is a **packaged Kubernetes application**.

```
myapp/
├── Chart.yaml
├── values.yaml
├── charts/
├── templates/
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── _helpers.tpl
│   └── NOTES.txt
├── .helmignore
```

---

## 1️⃣ Chart.yaml (MANDATORY)

📌 **Metadata about the chart**

```yaml
apiVersion: v2
name: myapp
description: A Helm chart for Kubernetes
type: application
version: 0.1.0
appVersion: "1.0"
```

### Key fields

| Field      | Purpose                        |
| ---------- | ------------------------------ |
| apiVersion | v2 for Helm 3                  |
| name       | Chart name                     |
| version    | Chart version (Helm uses this) |
| appVersion | App version (informational)    |
| type       | application / library          |

📌 **Interview point**

> `version` is for Helm, `appVersion` is for the application.

---

## 2️⃣ values.yaml (DEFAULT CONFIG)

📌 **User-configurable values**

```yaml
replicaCount: 2

image:
  repository: nginx
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
```

* Can be overridden using:

  ```bash
  helm install myapp ./myapp -f custom-values.yaml
  ```

  or

  ```bash
  helm install myapp ./myapp --set replicaCount=3
  ```

---

## 3️⃣ templates/ (K8s MANIFESTS)

📌 Contains **Go-templated Kubernetes YAML files**

### Example: deployment.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "myapp.fullname" . }}
spec:
  replicas: {{ .Values.replicaCount }}
```

### Common template files

| File            | Purpose            |
| --------------- | ------------------ |
| deployment.yaml | Creates Deployment |
| service.yaml    | Creates Service    |
| ingress.yaml    | Creates Ingress    |
| configmap.yaml  | Creates ConfigMap  |
| secret.yaml     | Creates Secret     |

---

## 4️⃣ _helpers.tpl (REUSABLE TEMPLATES)

📌 **Template functions / helpers**

```yaml
{{- define "myapp.fullname" -}}
{{ .Release.Name }}-{{ .Chart.Name }}
{{- end }}
```

Used as:

```yaml
name: {{ include "myapp.fullname" . }}
```

📌 Helps avoid duplication

---

## 5️⃣ charts/ (DEPENDENCIES)

📌 Stores **sub-charts**

Example:

```
charts/
└── redis/
```

Or defined in `Chart.yaml`:

```yaml
dependencies:
  - name: redis
    version: 17.0.0
    repository: https://charts.bitnami.com/bitnami
```

Fetched using:

```bash
helm dependency update
```

---

## 6️⃣ NOTES.txt (POST-INSTALL MESSAGE)

📌 Displayed after install

```txt
Your application is running.

Access it using:
kubectl port-forward svc/{{ include "myapp.fullname" . }} 8080:80
```

---

## 7️⃣ .helmignore

📌 Like `.gitignore`

```
*.tgz
*.bak
.git/
```

---

## 8️⃣ How Helm uses this structure (FLOW)

```text
helm install
 ↓
reads Chart.yaml
 ↓
loads values.yaml + overrides
 ↓
renders templates/
 ↓
creates Kubernetes resources
```

---

## 9️⃣ Minimal Helm Chart (exam-ready)

```
mychart/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── deployment.yaml
    └── service.yaml
```

---

## 🔟 Interview-ready summary 🚀

> A Helm chart consists of metadata in Chart.yaml, configurable values in values.yaml, Kubernetes manifests in templates/, optional dependencies in charts/, and reusable template helpers in _helpers.tpl.

---
# values-file

**Override values in values file**

- To override the values in values.yml file

```
helm install --set wordpressname="helm tutorials" my-release bitnami/wordpress
```

- If too many values need to be changed , then we can use our custom values file. however instead of = we need to use :, since its a yaml file

cat custom-values.yaml
```yaml
wordpressname="helm tutorials"
wordpressemail="seshaec"
```
```
helm install --values custom-values.yaml my-release bitnami/wordpress
```

**Use values file without overriding**

- First pull the chart and uncompress it using the below command. Because if you only pull, then it will create a chart in compressed state. so we need to uncompress it.

```
helm pull --untar bitnami/wordpress
```

## 1️⃣ `helm pull bitnami/wordpress`

### What this command does

* Downloads the chart **as a packaged archive**
* Does **NOT extract** it

### Files created

```bash
$ ls
wordpress-24.0.3.tgz
```

📌 (`24.0.3` is an example version — yours may differ)

### What it means

* `.tgz` = compressed Helm chart
* Useful for:

  * Offline installs
  * Sharing charts
  * Version pinning



## 2️⃣ `helm pull --untar bitnami/wordpress`

### What this command does

* Downloads **and extracts** the chart

### Files created

```bash
$ ls
wordpress/
```

### Inside the directory

```bash
$ ls wordpress
Chart.yaml
values.yaml
charts/
templates/
README.md
LICENSE
```

### And inside `templates/`

```bash
$ ls wordpress/templates
deployment.yaml
service.yaml
ingress.yaml
pvc.yaml
secrets.yaml
_helpers.tpl
NOTES.txt
```



## 3️⃣ Side-by-side comparison

| Command                               | `ls` output            |
| ------------------------------------- | ---------------------- |
| `helm pull bitnami/wordpress`         | `wordpress-24.x.x.tgz` |
| `helm pull --untar bitnami/wordpress` | `wordpress/`           |



## 4️⃣ How Helm normally uses these

### Using `.tgz`

```bash
helm install wp wordpress-24.0.3.tgz
```

### Using extracted directory

```bash
helm install <release-name> ./wordpress
```


## 5️⃣ Interview-ready one-liner 🚀

> `helm pull` downloads a packaged chart, while `helm pull --untar` downloads and extracts the chart into a directory.

---



