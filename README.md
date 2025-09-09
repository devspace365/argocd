# How to Install and Access ArgoCD on Your Local Cluster

If you’re working with Kubernetes and looking to streamline GitOps workflows, ArgoCD is one of the most powerful tools you can add to your toolkit. It allows you to manage deployments declaratively, keeping your applications and infrastructure in sync with your Git repositories.

In this quick guide, I’ll walk you through how to install ArgoCD on your cluster and access it locally in your browser. Let’s dive in! 🚀

## Prerequisites

kind
kubectl
kubernetes


## 1. Install ArgoCD on your Cluster

First, create a namespace for ArgoCD and apply the installation manifest:
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

This will spin up all the necessary ArgoCD components inside the argocd namespace.

## 2. Verify the Pods

Once the installation starts, verify that the ArgoCD pods are running properly. Use the following command to watch their status in real-time:
```
kubectl get pods -n argocd -w
```

Ensure all pods show the status Running.

If you see PodInitializing, wait until the status changes to Running before proceeding to the next section.

## 3. Expose the ArgoCD Server

By default, the ArgoCD server runs as a ClusterIP service, which isn’t accessible outside the cluster. To make it reachable in your browser, you need to change it to NodePort:
```
kubectl edit svc argocd-server -n argocd
```

Save and exit, and Kubernetes will update the service type.

## 4. Retrieve the Admin Password

To list all secrets in the argocd namespace, the command is:
```
kubectl get secret -n argocd
```
To log in to the ArgoCD dashboard, you’ll need the initial admin password. First, grab the secret:
```
kubectl edit secret argocd-initial-admin-secret -n argocd
```

Inside the YAML output, you’ll find an encoded string under password. Copy it.

## 5. Decode the Password

The password is base64 encoded, so let’s decode it with a simple command:
```
echo 10305ENCODEDPASSWORD6080== | base64 --decode
```

Replace the string above with your copied value. This gives you the plain-text password needed for login.

## 6. Access ArgoCD UI "User Interface" in your browser 

Finally, to open ArgoCD in your browser, forward the service port:
```
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

Now you can head to https://localhost:8080
, log in with the username admin and your decoded password, and start managing your apps with ArgoCD! 🎉

Wrapping Up

And that’s it — in just a few steps, you’ve installed ArgoCD, accessed it locally, and are ready to embrace GitOps workflows. From here, you can start connecting your repositories and managing deployments with confidence.

Have you tried running ArgoCD in your own cluster yet? If so, what was the biggest win you noticed?

Article by:

Shiller Calixte 
@ DevSpace365
