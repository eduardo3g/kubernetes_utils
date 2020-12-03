# k8s Survival Guide
Useful Kubernetes CLI commands 👷🏽

## Secret keys

[X] Create a secret inside a Kubernetes cluster. For example, your application uses a Stripe integration that requires a secret key. However, you don't want to write it in plain text in your code. To solve that you create a secret key inside your Kubernetes cluster, then you'll be able to access them as environment variables.

```
kubectl create secret generic [secret-name] --from-literal [key]=[value]
```
For example:
```
kubectl create secret generic stripe-secret --from-literal STRIPE_KEY=sk_test_fakestripesecretkey
```
With that in hands, you can add it to your deployment file, inside the env key of container specs.
```
      containers:
        - name: payments
          image: dockerid/dockerimage
          env:
            - name: STRIPE_KEY
              valueFrom:
                secretKeyRef:
                  name: stripe-secret
                  key: STRIPE_KEY     
```

It will allow you to access this environment variable with ```process.env.STRIPE_KEY```

[X] Get existing secrets
```
kubectl get secrets
```

## Contexts

[X] Imagine you're running a local Kubernetes cluster in your local machine with Minikube. However, you also have Kubernetes cluster deployed at Digital Ocean. Eventually, you'll want to switch between contexts, that is, your local context or from Digital Ocean. To do so, follow the steps below:

1. First, connect to your Digital Ocean cluster with the following command: ```doctl kubernetes cluster kubeconfig save <CLUSTER_NAME>```

2. ``` kubectl config view ```: inside of the key contexts there's an array of available contexts.
```
contexts:
- context:
    cluster: my-digitalocean-cluster
    user: my-digitalocean-admin
  name: my-digitalocean-app
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube  
```
In the example above the current context is <b>minikube</b>. If you want to switch to Digital Ocean's context, run the following command:

3. ``` kubectl config use-context my-digitalocean-cluster ```: Once you've done that, run ```kubectl get nodes``` to see the response from Digital Ocean.

## Basics
[X] Run shell commands inside a pod: ```kubectl -it exec <POD_NAME> sh```. For example:
```
kubectl -it exec webapp sh
```
