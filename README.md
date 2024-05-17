# Tutorial 11

## Reflection on Hello Minikube

1. Compare the application logs before and after you exposed it as a Service. What do you see in the logs? Does the number of logs increase each time you open the app?

**Before Exposure**

<img src="image/Screenshot 2024-05-15 at 18.34.35.png">

**After Exposure**

<img src="image/Screenshot 2024-05-15 at 18.35.33.png">

After exposing the application as a service, the logs not only show the initial startup sequence but also include multiple GET / requests. Each entry corresponds to a new interaction or request received by the server. As a result, the number of log entries increases, indicating that the application is now actively handling incoming HTTP requests.

2. Regarding the kubectl get command, the -n option is used to specify a namespace, allowing you to filter resources within a Kubernetes cluster. For example, using kubectl get services without the -n option defaults to the default namespace, showing resources like hello-node as seen in tutorials. In contrast, using kubectl get pods, services -n kube-system targets the kube-system namespace, which contains critical system components. Therefore, any pods or services you created, likely in the default namespace, won't appear in the output from the latter command.

## Reflection 2
### 1. What is the difference between Rolling Update and Recreate deployment strategy?
Rolling Update: This strategy is commonly utilized by Kubernetes deployments for updating applications. It replaces old pods with new ones progressively, ensuring continuous availability without downtime. The process involves gradually decreasing the count of old replicas while simultaneously increasing the new replicas, thus maintaining operational service during the update.

Recreate: This method removes all existing pods before generating new ones. Contrary to the Rolling Update, the Recreate strategy leads to downtime because there are periods when no application instances are active. Nonetheless, it's beneficial in situations where running two versions of the application at the same time is not possible.

### 2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document

<img src="image/Screenshot 2024-05-18 at 00.56.16.png">

### 3. Prepare different manifest files for executing Recreate deployment strategy.

```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
    annotations:
    deployment.kubernetes.io/revision: "1"
    kubectl.kubernetes.io/last-applied-configuration: |
        {"apiVersion":"apps/v1","kind":"Deployment","metadata":{"annotations":{"deployment.kubernetes.io/revision":"4"},"creationTimestamp":"2024-05-14T06:34:29Z","generation":6,"labels":{"app":"spring-petclinic-rest"},"name":"spring-petclinic-rest","namespace":"default","resourceVersion":"5181","uid":"0e6e41f6-bf46-4d4f-a4ed-82f1ca59b27e"},"spec":{"progressDeadlineSeconds":600,"replicas":4,"revisionHistoryLimit":10,"selector":{"matchLabels":{"app":"spring-petclinic-rest"}},"strategy":{"type":"Recreate"},"template":{"metadata":{"creationTimestamp":null,"labels":{"app":"spring-petclinic-rest"}},"spec":{"containers":[{"image":"docker.io/springcommunity/spring-petclinic-rest:3.2.1","imagePullPolicy":"IfNotPresent","name":"spring-petclinic-rest","resources":{},"terminationMessagePath":"/dev/termination-log","terminationMessagePolicy":"File"}],"dnsPolicy":"ClusterFirst","restartPolicy":"Always","schedulerName":"default-scheduler","securityContext":{},"terminationGracePeriodSeconds":30}}},"status":{"conditions":[{"lastTransitionTime":"2024-05-14T06:34:29Z","lastUpdateTime":"2024-05-14T07:03:39Z","message":"ReplicaSet \"spring-petclinic-rest-54f476f68\" has successfully progressed.","reason":"NewReplicaSetAvailable","status":"True","type":"Progressing"},{"lastTransitionTime":"2024-05-14T07:14:04Z","lastUpdateTime":"2024-05-14T07:14:04Z","message":"Deployment does not have minimum availability.","reason":"MinimumReplicasUnavailable","status":"False","type":"Available"}],"observedGeneration":6,"replicas":4,"unavailableReplicas":4,"updatedReplicas":4}}
    creationTimestamp: "2024-05-14T12:57:25Z"
    generation: 1
    labels:
    app: spring-petclinic-rest
    name: spring-petclinic-rest
    namespace: default
    resourceVersion: "15734"
    uid: b7ddb571-2ea7-4511-a8b0-c2b7e5de196a
    spec:
    progressDeadlineSeconds: 600
    replicas: 4
    revisionHistoryLimit: 10
    selector:
    matchLabels:
        app: spring-petclinic-rest
    strategy:
    type: Recreate
    template:
    metadata:
        creationTimestamp: null
        labels:
        app: spring-petclinic-rest
    spec:
        containers:
        - image: docker.io/springcommunity/spring-petclinic-rest:3.2.1
        imagePullPolicy: IfNotPresent
        name: spring-petclinic-rest
        resources: {}
        terminationMessagePath: /dev/termination-log
        terminationMessagePolicy: File
        dnsPolicy: ClusterFirst
        restartPolicy: Always
        schedulerName: default-scheduler
        securityContext: {}
        terminationGracePeriodSeconds: 30
    status:
    conditions:
    - lastTransitionTime: "2024-05-14T12:57:25Z"
    lastUpdateTime: "2024-05-14T12:57:25Z"
    message: Deployment does not have minimum availability.
    reason: MinimumReplicasUnavailable
    status: "False"
    type: Available
    - lastTransitionTime: "2024-05-14T12:57:25Z"
    lastUpdateTime: "2024-05-14T12:57:26Z"
    message: ReplicaSet "spring-petclinic-rest-54f476f68" is progressing.
    reason: ReplicaSetUpdated
    status: "True"
    type: Progressing
    observedGeneration: 1
    replicas: 4
    unavailableReplicas: 4
    updatedReplicas: 4
```

### 4. What do you think are the benefits of using Kubernetes manifest files?
Consistency: Using manifest files helps maintain consistency, reproducibility, and allows for version control in deployments. This approach reduces the risk of errors that might arise from manual commands.

Automation: Deploying with a manifest file (via kubectl apply -f) automates the setup process, streamlining the deployment of the application along with its associated configurations, secrets, and network policies.

Scalability: Manifest files simplify the scaling process. Modifications can be made directly in the file and then reapplied, facilitating easy adjustments to resource allocation.

Documentation: These files serve as a comprehensive record of the deployment's current state, providing crucial information for ongoing maintenance and a clear understanding of the setup.





