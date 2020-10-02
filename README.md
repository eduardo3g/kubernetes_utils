# kubernetes_utils
Useful Kubernetes CLI commands 👷🏽

## Secret keys

[X] Create a secret inside a Kubernetes cluster
For example, you application uses a Stripe integration that requires a Stripe secret key. However, you don't want to write it in plain text in your code.

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
