## java-app-demo

### volume.yaml
把挂载目录部分单独一个yaml 文件进行配置;

### 将挂载目录配置写入同一个文件的配置如下:
> app-agent-demo-deploy.yml

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-agent-demo
  namespace: test-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: app-agent-demo
  template:
    metadata:
      labels:
        app: app-agent-demo
    spec:
      initContainers:
        - name: init
          image: reg.jevic.cn/basic/skywalking-agent:v7
          command:
            [
              "sh",
              "-c",
              "set -ex;mkdir -p /skywalking/agent && cp -a /opt/skywalking/agent/* /skywalking/agent/",
            ]
          volumeMounts:
            - name: agent
              mountPath: /skywalking/agent
      containers:
        - name: app-agent-demo
          image: registry.cn-shenzhen.aliyuncs.com/jevic/skywalking:java-app-demo
          command: ["/sh/init.sh"]
          volumeMounts:
          - name: agent
            mountPath: /skywalking/agent
          resources:
            limits:
              cpu: 1000m
              memory: 1Gi
            requests:
              cpu: 100m
              memory: 1Gi
          ports:
            - containerPort: 8080
          volumeMounts:
            - name: sh
              mountPath: /sh
              readOnly: false
      volumes:
        - name: agent
          emptyDir: {}
        - name: sh
          configMap:
            name: env-config-map
            defaultMode: 0755
            items:
              - key: init.sh
                path: init.sh
```
