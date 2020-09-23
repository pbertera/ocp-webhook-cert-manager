# OpenShift Webhook Certificate Manager

Script to generate a certificate suitable for use with any Kubernetes Mutating or Validating Webhook.

This is a detailed list of steps the script is executing:

- Generate a server key.
- If there is any previous CSR (certificate signing request) for this key, it is deleted.
- Generate a CSR for such key.
- The signature of the key is then approved.
- The server's certificate is fetched from the CSR and then encoded.
- A secret of type tls is created with the server certificate and key.
- The k8s extension api server's CA bundle is fetched.
- The mutating webhook configuration for the webhook server is patched with the k8s api server's CA bundle from the previous step. This CA bundle will be used by the k8s extension api server when calling our webhook.

If you wish to learn more about TLS certificates management inside Kubernetes, check out the official documentation for [Managing TLS Certificate in a Cluster](https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/#create-a-certificate-signing-request-object-to-send-to-the-kubernetes-api).

## Usage example

The script expects multiple mandatory arguments. This is an example:

``` sh
./generate_certificate.sh --service ${WEBHOOK_SERVICE_NAME} --webhook ${WEBHOOK_NAME} --secret ${SECRET_NAME} --namespace ${WEBHOOK_NAMESPACE}
``` 

## Credits

This script is derived from the New Relic project [k8s-webhook-cert-manager](https://github.com/newrelic/k8s-webhook-cert-manager) project and adapted to be used out of a Pod in an OpenShift cluster.
