1. How to deploy the app to k8s.
First you need apply manifests
kubectl apply -f deployment.yml
kubectl apply -f hpa.yml

2. I chose these values because they are sufficient for this app in idle state. The Django ToDo app is a lightweight application with minimal resource requirements. 64Mi memory and 250m CPU are enough to run 2 pods smoothly. Limits are set 2x higher to handle occasional spikes without OOM kills.

3. I choose Horizontal version because it's task exersice. Values 70 I used with formula:
Desired Replicas=Current Replicas×(Current Metric Value/Target Metric Value)

4. Number minReplicas tell us how minimum pods we should use. maxReplicas it's maximal number of pods we can create there. If load cpu will be more than 70% service create more pods.

5. After deployment, the app won't be accessible externally without a Service. You can temporarily access it using port-forward:
kubectl port-forward -n mateapp deployment/todoapp 8081:8080
Then open http://localhost:8081"