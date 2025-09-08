RC 控制器
apiVersion: v1
kind: ReplicationController
metadata:
  name: rc-demo
spec:
  replicas: 3
  selector:
    app: rc-demo
  template:
    metadata:
      labels:
        app: rc-demo
    spec:
      containers:
        - name: rc-demo-container
          image: wangyanglinux/myapp:v1.0
          env:
            - name: GET_HOSTS_FROM
              value: dns
            - name: zhangsan  # 注意：此处应为独立的环境变量项
              value: "123"
          ports:
            - containerPort: 80