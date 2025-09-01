# argocd
How to install ArgoCD on your local kubernetes cluster and introduction to ArgoCD
How to Install and Access ArgoCD on Your Local Kubernetes Cluster

If you’re working with Kubernetes and looking to streamline GitOps workflows, ArgoCD is one of the most powerful tools you can add to your toolkit. It allows you to manage deployments declaratively, keeping your applications and infrastructure in sync with your Git repositories.

In this quick guide, I’ll walk you through how to install ArgoCD on your cluster and access it locally in your browser. Let’s dive in! 🚀

## 1. Install ArgoCD

First, create a namespace for ArgoCD and apply the installation manifest:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This will spin up all the necessary ArgoCD components inside the argocd namespace.

## 2. Verify the Pods

Once the installation starts, you’ll want to make sure the pods are running properly. Use the following command to watch their status in real-time:
```
kubectl get pods -n argocd -w
```

Wait until all pods show a Running or Completed status before moving forward.

## 3. Expose the ArgoCD Server

By default, the ArgoCD server runs as a ClusterIP service, which isn’t accessible outside the cluster. To make it reachable in your browser, you need to change it to NodePort:
```
kubectl edit svc argocd-server -n argocd
```

Save and exit, and Kubernetes will update the service type.

## 4. Retrieve the Admin Password

To log in to the ArgoCD dashboard, you’ll need the initial admin password. First, grab the secret:
```
kubectl get secret -n argocd
kubectl edit secret argocd-initial-admin-secret -n argocd
```

Inside the YAML output, you’ll find an encoded string under password. Copy it.

## 5. Decode the Password

The password is base64 encoded, so let’s decode it with a simple command:
```
echo WTlRWE90aTlubWR5dzhjUA== | base64 --decode
```

Replace the string above with your copied value. This gives you the plain-text password needed for login.

6. Port Forward to Your Localhost

Finally, to open ArgoCD in your browser, forward the service port:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Now you can head to https://localhost:8080
, log in with the username admin and your decoded password, and start managing your apps with ArgoCD! 🎉

Wrapping Up

And that’s it — in just a few steps, you’ve installed ArgoCD, accessed it locally, and are ready to embrace GitOps workflows. From here, you can start connecting your repositories and managing deployments with confidence.

Have you tried running ArgoCD in your own cluster yet? If so, what was the biggest win you noticed?
