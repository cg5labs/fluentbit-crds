Here is a comprehensive `README.md` for your **Fluent Bit Operator Helm chart**, supporting both **cluster-scoped** and **namespaced CRDs**:

---

### 📄 `README.md`

````markdown
# Fluent Bit Operator CRDs Helm Chart

This Helm chart installs Kubernetes CRDs for the [Fluent Bit Operator](https://github.com/fluent/fluent-operator), including support for:

- ✅ Cluster-level CRDs: `ClusterInput`, `ClusterFilter`, `ClusterOutput`, `ClusterParser`
- ✅ Namespaced CRDs: `Input`, `Filter`, `Output`, `Parser`
- 🔁 Multiple entries per CRD type, each defined independently

---

## 📦 Installation

```bash
helm install fluentbit-crds ./fluentbit-operator-crds
````

To install with a custom configuration:

```bash
helm install fluentbit-crds ./fluentbit-operator-crds -f values.yaml
```

To upgrade:

```bash
helm upgrade fluentbit-crds ./fluentbit-operator-crds -f values.yaml
```

To uninstall:

```bash
helm uninstall fluentbit-crds
```

---

## ⚙️ Configuration

All CRDs are configured in `values.yaml` as lists. Each list item defines one CRD instance. If the list is empty or omitted, the CRD is not created.

### Cluster Inputs

| Key             | Type | Description                      |
| --------------- | ---- | -------------------------------- |
| `clusterInputs` | list | List of `ClusterInput` resources |

Example:

```yaml
clusterInputs:
  - name: tail-containers
    input:
      tail:
        Tag: kube.*
        Path: /var/log/containers/*.log
        Parser: docker
```

---

### Cluster Filters

| Key              | Type | Description                       |
| ---------------- | ---- | --------------------------------- |
| `clusterFilters` | list | List of `ClusterFilter` resources |

Example:

```yaml
clusterFilters:
  - name: kubernetes-meta
    filter:
      kubernetes:
        Match: kube.*
        Merge_Log: On
```

---

### Cluster Outputs

| Key              | Type | Description                       |
| ---------------- | ---- | --------------------------------- |
| `clusterOutputs` | list | List of `ClusterOutput` resources |

Example:

```yaml
clusterOutputs:
  - name: loki-output
    output:
      loki:
        Match: kube.*
        Url: http://loki-gateway/loki/api/v1/push
```

---

### Cluster Parsers

| Key              | Type | Description                       |
| ---------------- | ---- | --------------------------------- |
| `clusterParsers` | list | List of `ClusterParser` resources |

Example:

```yaml
clusterParsers:
  - name: json-custom
    parser:
      format: json
      time_key: time
      time_format: "%Y-%m-%dT%H:%M:%S"
```

---

## 🗂️ Namespaced CRDs

Each namespaced CRD requires a `name` and a `namespace`.

### Inputs

| Key      | Type | Description               |
| -------- | ---- | ------------------------- |
| `inputs` | list | List of `Input` resources |

Example:

```yaml
inputs:
  - name: tail-app
    namespace: logging
    input:
      tail:
        Path: /var/log/app/*.log
```

---

### Filters

| Key       | Type | Description                |
| --------- | ---- | -------------------------- |
| `filters` | list | List of `Filter` resources |

Example:

```yaml
filters:
  - name: enrich
    namespace: logging
    filter:
      modify:
        Match: app.*
        Add:
          env: prod
```

---

### Outputs

| Key       | Type | Description                |
| --------- | ---- | -------------------------- |
| `outputs` | list | List of `Output` resources |

Example:

```yaml
outputs:
  - name: stdout
    namespace: logging
    output:
      stdout:
        Match: app.*
```

---

### Parsers

| Key       | Type | Description                |
| --------- | ---- | -------------------------- |
| `parsers` | list | List of `Parser` resources |

Example:

```yaml
parsers:
  - name: custom-json
    namespace: logging
    parser:
      format: json
      time_key: timestamp
```

---

## 🧪 Testing

To preview what will be rendered without applying:

```bash
helm template fluentbit-crds ./fluentbit-operator-crds
```

---

## ❗ Requirements

* The **Fluent Bit Operator** must already be installed on the cluster.
* This chart **only installs CRDs**, not the operator itself.

---

## 📜 License

Apache 2.0

---

## 🤝 Contributions

PRs and issues welcome!

