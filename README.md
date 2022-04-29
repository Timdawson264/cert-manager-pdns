### Deployment

Deploy the custom pdns apiextenion using the helm chart in depploy.

This is how i deployed it on openshift.
certManager.namespace should match the namespace that the cert-manager pod + service account are running on your cluster.
Std openshift + comunity cert-manager running in openshift-operators namespace.

```
helm install --create-namespace -n pdns-webhook --set certManager.namespace="openshift-operators" pdns-webhook ./deploy/pdns-webhook
```

### Example Issuer using the staging letsencypt api. 

```
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: dns-acme-issuer
spec:
  acme:
    email: user@example.com
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    privateKeySecretRef:
      name: acme-account-secret
    solvers:
    - dns01:
        webhook:
          groupName: acme.powerdns.com
          solverName: powerdns
          config:
            server: "http://powerdnsserverurl:80"
            apikey: supersecret
```


