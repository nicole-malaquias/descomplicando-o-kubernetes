## Objective

Create a simple application Pod that hosts a web application.

## Requirements

1. Use a Docker image of your choice to create a container in the Pod (can be a web application in Node.js, Python, PHP, etc.).
2. Define at least one CPU and one memory resource for the container.
3. Open a port in the container (e.g. port 80) to access the application.
4. Configure the Pod to use the "ClusterFirst" DNS policy.
5. Define the name of the Pod according to Kubernetes naming conventions.
6. Use appropriate labels to identify the Pod.

Tip: You can use a simple web server, such as Nginx or Apache, as the application for your container.

After creating the Pod manifest, you can apply it to your Kubernetes cluster using **`kubectl apply -f your-file.yaml`**. Make sure the Pod is running and accessible in a web browser or using **`kubectl port-forward`**.

This challenge will help you practice creating Pod manifests with CPU and memory resources, port configuration, label usage, and DNS policy application.