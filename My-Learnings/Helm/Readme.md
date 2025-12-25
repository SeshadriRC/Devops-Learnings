- [Notes](#notes)
- [Install helm](#Install-helm)
- [Helm-Chart-Structure](#helm-chart-structure)
- [values-file](#values-file)
- [Practicals](#Practicals)


# Notes

- if you give rollback, then it won't take the old state of database. only it will affect kubernetes resources

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

# Practicals

1. Which command is used to search for a wordpress helm chart package from the Artifact Hub?

Run helm search hub chart-name command to search specific charts on Artifact Hub.

Note: Replace the chart-name with the necessary package.

```
helm search hub chart-name
Eg: helm search hub wordpress
```

2. Search for a consul helm chart package from the Artifact Hub and identify the APP VERSION for the Official HashiCorp Consul Chart.

Run the command helm search hub consul | grep hashicorp and check the APP VERSION.

```
helm search hub consul | grep hashicorp
```

3. Add bitnami helm chart repository in the controlplane node.


The url for bitnami repository is https://charts.bitnami.com/bitnami

```
helm repo add bitnami https://charts.bitnami.com/bitnami

controlplane ~ ➜  helm repo add bitnami https://charts.bitnami.com/bitnami
"bitnami" has been added to your repositories

controlplane ~ ➜  helm repo list
NAME    URL                               
bitnami https://charts.bitnami.com/bitnami
```

4. Which command is used to search for the wordpress package from the newly added bitnami repository?

Run helm search repo wordpress command to search specific package from all the added repository.
```
helm search repo wordpress
```

5. How many helm chart repositories are there in the controlplane node now?

```
controlplane ~ ➜  helm repo list
NAME            URL                                                 
bitnami         https://charts.bitnami.com/bitnami                  
puppet          https://puppetlabs.github.io/puppetserver-helm-chart
hashicorp       https://helm.releases.hashicorp.com         
```

6. Deploy the Apache application on the cluster using the apache from the bitnami repository.


Set the release Name to: amaze-surf

```
controlplane ~ ➜  helm search repo apache
NAME                            CHART VERSION   APP VERSION     DESCRIPTION                                       
bitnami/apache                  11.4.29         2.4.65          Apache HTTP Server is an open-source HTTP serve...
bitnami/airflow                 25.0.2          3.0.5           Apache Airflow is a tool to express and execute...
bitnami/apisix                  6.0.0           3.13.0          Apache APISIX is high-performance, real-time AP...
bitnami/cassandra               12.3.11         5.0.5           Apache Cassandra is an open source distributed ...
bitnami/dataplatform-bp2        12.0.5          1.0.1           DEPRECATED This Helm chart can be used for the ...
bitnami/flink                   2.0.7           2.1.0           Apache Flink is a framework and distributed pro...
bitnami/geode                   1.1.8           1.15.1          DEPRECATED Apache Geode is a data management pl...
bitnami/influxdb                7.1.20          3.4.1           InfluxDB(TM) Core is an open source time-series...
bitnami/kafka                   32.4.3          4.0.0           Apache Kafka is a distributed streaming platfor...
bitnami/mxnet                   3.5.2           1.9.1           DEPRECATED Apache MXNet (Incubating) is a flexi...
bitnami/schema-registry         26.0.5          8.0.0           Confluent Schema Registry provides a RESTful


helm install amaze-surf bitnami/apache

controlplane ~ ➜  helm install amaze-surf bitnami/apache
Pulled: us-central1-docker.pkg.dev/kk-lab-prod/helm-charts/bitnami/apache:11.3.2
Digest: sha256:1bd45c97bb7a0000534e3abc5797143661e34ea7165aa33068853c567e6df9f2
NAME: amaze-surf
LAST DEPLOYED: Thu Dec 25 09:14:21 2025
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: apache
CHART VERSION: 11.3.2
APP VERSION: 2.4.63
```

7. What version of apache did we just install on the cluster using the helm chart?

- Run the command helm list and check the APP VERSION of the release.
```
controlplane ~/apache/templates ➜  helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
amaze-surf      default         1               2025-12-25 09:14:21.002394696 +0000 UTC deployed        apache-11.3.2   2.4.63     
```

8. How many releases of nginx charts can you see installed in the cluster now?

Note: We just installed some charts
Run the command helm list and check the number of installed nginx charts.

```
controlplane ~/apache/templates ➜  helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
amaze-surf      default         1               2025-12-25 09:14:21.002394696 +0000 UTC deployed        apache-11.3.2   2.4.63     
crazy-web       default         1               2025-12-25 09:23:05.844657386 +0000 UTC deployed        nginx-19.0.0    1.27.4     
happy-browse    default         1               2025-12-25 09:23:03.179129536 +0000 UTC deployed        nginx-19.0.0    1.27.4     
```

9. Uninstall the nginx chart release happy-browse from the cluster.

Run the command helm uninstall <release-name> to uninstall the chart.

```
helm uninstall <release-name>

controlplane ~/apache/templates ➜  helm uninstall happy-browse
release "happy-browse" uninstalled

controlplane ~/apache/templates ➜  helm ls
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
amaze-surf      default         1               2025-12-25 09:14:21.002394696 +0000 UTC deployed        apache-11.3.2   2.4.63     
crazy-web       default         1               2025-12-25 09:23:05.844657386 +0000 UTC deployed        nginx-19.0.0    1.27.4     
```

10. Remove the Hashicorp helm repository from the cluster.

Run the following command:

```
helm repo remove hashicorp

controlplane ~/apache/templates ➜  helm repo remove hashicorp
"hashicorp" has been removed from your repositories

controlplane ~/apache/templates ➜  helm repo ls
NAME    URL                                                 
bitnami https://charts.bitnami.com/bitnami                  
puppet  https://puppetlabs.github.io/puppetserver-helm-chart
```

11. How many revisions of nginx exists in the cluster?

```
helm history <release-name>

controlplane ~ ➜  helm history dazzling-web
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION     
1               Thu Dec 25 10:11:31 2025        superseded      nginx-12.0.4    1.22.0          Install complete
2               Thu Dec 25 10:11:33 2025        superseded      nginx-12.0.5    1.22.0          Upgrade complete
3               Thu Dec 25 10:11:34 2025        deployed        nginx-12.0.4    1.22.0          Upgrade complete
```
In Helm, superseded simply means:

“This revision is no longer the active one because a newer revision replaced it.”

12. The DevOps team has decided to upgrade the nginx version to 1.27.x and use the Helm chart version 18.3.6 from the Bitnami repository.


Ensure that the nginx version running in the cluster is 1.27.x.

To upgrade to nginx version 1.27.x using helm chart version 18.3.6, run the following command: This will install the nginx version 1.27.x.

```
helm upgrade <release-name> <chart-name> --version 18.3.6
helm upgrade dazzling-web bitnami/nginx --version 18.3.6
```

13. Oops!.. There seems to be a minor issue in the website and the DevOps Team is asked to rollback the nginx to previous version!


Please rollback the nginx to previous version.

```
helm rollback dazzling-web


controlplane ~ ➜  helm rollback dazzling-web 1
Rollback was a success! Happy Helming!

controlplane ~ ➜  helm history dazzling-web
REVISION        UPDATED                         STATUS          CHART           APP VERSION     DESCRIPTION     
1               Thu Dec 25 10:11:31 2025        superseded      nginx-12.0.4    1.22.0          Install complete
2               Thu Dec 25 10:11:33 2025        superseded      nginx-12.0.5    1.22.0          Upgrade complete
3               Thu Dec 25 10:11:34 2025        superseded      nginx-12.0.4    1.22.0          Upgrade complete
4               Thu Dec 25 10:17:52 2025        superseded      nginx-18.3.6    1.27.4          Upgrade complete
5               Thu Dec 25 10:19:15 2025        superseded      nginx-12.0.4    1.22.0          Rollback to 3   
6               Thu Dec 25 10:20:03 2025        deployed        nginx-12.0.4    1.22.0          Rollback to 1   

```

