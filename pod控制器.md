控制器
	Pod 控制器
		守护进程类型
			RC 控制器
				保障当前的 Pod 数量与期望值一致
            RS 控制器
				功能与 RC 控制器类似，但是多了标签选择的运算方式


RC 控制器 使用案例，编写资源清单
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

##修改rc规模
kubectl scale rc rc-demo --replicas=10

RS控制器
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs-ml-demo
spec:
  replicas: 3
  selector:
    matchLabels:
      app: rs-ml-demo
  template:
    metadata:
      labels:
        app: rs-ml-demo
    spec:
      containers:
        - name: rs-ml-demo-container
          image: wangyanglinux/myapp:v1.0
          env:
            - name: GET_HOSTS_FROM
              value: dns
          ports:
            - containerPort: 80

rs 在标签选择器上，除了可以定义键值对的选择形式，还支持 matchExpressions 字段，可以提供多种
选择。
目前支持的操作包括：
In：label 的值在某个列表中
NotIn：label 的值不在某个列表中
Exists：某个 label 存在
DoesNotExist：某个 label 不存在
示例：
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs-me-exists-demo
spec:
  selector:
    matchExpressions:
      - key: app
        operator: Exists
  template:
    metadata:
      labels:
        app: spring-k8s
    spec:
      containers:
        - name: rs-me-exists-demo-container
          image: wangyanglinux/myapp:v1.0
          ports:
            - containerPort: 80


          ‌‍

apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: rs-me-in-demo
spec:
  selector:
    matchExpressions:
      - key: app
        operator: In
        values:
          - spring-k8s
          - hahahahs
  template:
    metadata:
      labels:
        app: sg-k8s
    spec:
      containers:
        - name: rs-me-in-demo-container
          image: wangyanglinux/myapp:v1.0
          ports:
            - containerPort: 80

