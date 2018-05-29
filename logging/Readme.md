# Kibana Ingress setup

1. Create secret

    ```
    htpasswd -c /tmp/auth bookworm
    ```

2. Upload secret to k8s:

    ```
    kubectl create secret generic kibana-credentials --from-file /tmp/auth --namespace=logging --context=denovo-prod
    ```
