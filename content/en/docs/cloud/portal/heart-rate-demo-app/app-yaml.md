---
title: "App YAML: Heart Rate Demo"
date: 2020-10-06T08:49:15+00:00
weight: 300
---

```yaml
kind: Application
apiVersion: iofog.org/v3
metadata:
  name: demo-app
spec:
  microservices:
    # Custom micro service that will connect to Scosche heart rate monitor via Bluetooth
    - name: "monitor"
      agent:
        name: "{% raw %}
{% assign agent = \"\" | findAgent | first %}{{ agent.name }}"
      images:
        arm: "edgeworx/healthcare-heart-rate:arm-v1"
        x86: "edgeworx/healthcare-heart-rate:x86-v1"
      container:
        rootHostAccess: false
        ports: []
      config:
        # data will be mocked
        test_mode: true
        data_label: "Anonymous Person"
    # Simple JSON viewer for the heart rate output
    - name: "viewer"
      agent:
        name: "{% assign agent = \"\" | findAgent | last %}
{% endraw %}{{ agent.name }}"
      images:
        arm: "edgeworx/healthcare-heart-rate-ui:arm"
        x86: "edgeworx/healthcare-heart-rate-ui:x86"
      container:
        rootHostAccess: false
        ports:
          # The ui will be listening on port 80 (internal).
          - external: 5000 # You will be able to access the ui on <AGENT_IP>:5000
            internal: 80 # The ui is listening on port 80. Do not edit this.
            public:
              schemes:
              - https
              protocol: http
        volumes: []
        env:
          - key: "BASE_URL"
            value: "http://localhost:8080/data"
  routes:
    # Use this section to configure route between microservices
    # Use microservice name
    - from: "{{self.microservices[0].name}}"
      to: "{{self.microservices[1].name}}"
      name: "monitor-to-viewer"
```