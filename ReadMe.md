# CredHub Service Broker for SpringBoot Demo

Use this demo to show how to leverage the TPCF [CredHub Service Broker](https://docs.vmware.com/en/CredHub-Service-Broker/services/credhub/GUID-index.html)
to store secret values that you want to access from within your application.

When you create your service, the secret will be stored securely within credhub.  The cloud foundry CAPI
will resolve reference stored in the app's VCAP_SERVICES using the mTLS certificates in the container.

1. Ensure you are logged into PAS and you have access to the credhub service from the marketplace

2. Build and push your app
```bash
./gradlew clean build
cf push
```

3. Create and bind the credhub service within your space.

```bash
cf create-service credhub default my-credhub-secret -c '{"SECRET_CREDENTIAL":"secret_value"}'
cf bind-service credhub-demo my-credhub-secret
```

4. Restage to make sure env variables are refreshed
```bash
cf restage credhub-demo
```

5.Test your app
```bash
curl https://<app_url>/secret/my-credhub-secret/SECRET_CREDENTIAL
```
You should expect the response to be `secret_value`.