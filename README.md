# Tutorial 11

## Reflection on Hello Minikube

1. Compare the application logs before and after you exposed it as a Service. What do you see in the logs? Does the number of logs increase each time you open the app?

**Before Exposure**

<img src="image/Screenshot 2024-05-15 at 18.34.35.png">

**After Exposure**

<img src="image/Screenshot 2024-05-15 at 18.35.33.png">

After exposing the application as a service, the logs not only show the initial startup sequence but also include multiple GET / requests. Each entry corresponds to a new interaction or request received by the server. As a result, the number of log entries increases, indicating that the application is now actively handling incoming HTTP requests.

2. Regarding the kubectl get command, the -n option is used to specify a namespace, allowing you to filter resources within a Kubernetes cluster. For example, using kubectl get services without the -n option defaults to the default namespace, showing resources like hello-node as seen in tutorials. In contrast, using kubectl get pods, services -n kube-system targets the kube-system namespace, which contains critical system components. Therefore, any pods or services you created, likely in the default namespace, won't appear in the output from the latter command.
