## Kubernetes Sample deployment file with explanation

    apiVersion: apps/v1             
    kind: Deployment           
    metadata:                        
      name: app-name
      labels:
        app: app-name
      namespace: default
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: app-name
      strategy:
        rollingUpdate:
          maxSurge: 25%
          maxUnavailable: 25%
        type: RollingUpdate
      template:
        metadata:
          labels:
            app: app-name
        spec:
          containers:
          - image: <image-name-with-path>
            imagePullPolicy: Always
            name: app-name
            ports:
            - containerPort: 80
              protocol: TCP
## Kubernetes Service file sample

    apiVersion: v1
    kind: Service
    metadata:
      name: app-name
      labels:
        app: app-name
      annotations:
        service.beta.kubernetes.io/aws-load-balancer-additional-resource-tags: "Name=app-name,environment=env"    
    spec:
      type: LoadBalancer
      selector:
        app: app-name
      ports:
      - port: 80
        name: http
        targetPort: 80
      - port: 443
        name: https
        targetPort: 80

## Define an environment variable for a containe

    apiVersion: v1
    kind: Deployment
    metadata:
      name: app-name
      labels:
        purpose: app-name
    spec:
      containers:
      - name: app-name
        image: <image-name>
        env:
        - name: ENV_VARIABLE1
          value: "Hello variable1"
        - name: ENV_VARIABLE2
          value: "Hello variable2"

## Service with internal loadbalancer

    [...]
    metadata:
        name: my-service
        annotations:
            service.beta.kubernetes.io/aws-load-balancer-internal: 0.0.0.0/0
    spec:
      type: LoadBalancer        
    [...]
