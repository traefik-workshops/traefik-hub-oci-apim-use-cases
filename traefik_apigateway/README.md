# Integrating Traefik API Gateway with Oracle Cloud Infrastructure (OCI) and Oracle Kubernetes Engine (OKE)

This document outlines the integration of Traefik API Gateway with OCI and OKE, focusing on enabling access to external OCI APIs and securing your setup.

> This is a continuation of the documentation:  
> https://doc.traefik.io/traefik-hub/operations/oracle-oci/oci-apim-marketplace

---

## Enable External APIs

By default, Traefik restricts access to services of type `ExternalName` and cross-namespace references for security reasons. Since this integration involves exposing APIs hosted outside your Kubernetes cluster (such as Oracle Integration Cloud endpoints), you must explicitly enable these features.

You can follow the steps in this guide:  
https://doc.traefik.io/traefik-hub/api-management/external-api

```bash
helm upgrade --install \
  --namespace traefik \
  traefik traefik/traefik \
  --reset-then-reuse-values \
  --set "providers.kubernetesCRD.allowExternalNameServices=true"
```

---

## Example Configuration: Exposing an External API via Traefik with API Gateway

**All referenced YAML files should be placed in your `/resources` folder in your GitHub repository.**

Replace `<External_IP>` with your Load Balancer IP:

```bash
export EXTERNAL_IP=$(kubectl get svc -n traefik traefik -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
echo "Use EXTERNAL_IP=${EXTERNAL_IP}"
```

### 1. Service and Routing Setup

#### 1.1 ExternalName Service for the API

Create the namespace:

```bash
kubectl create namespace apps
```

Apply the service that will expose inside OKE the OCI API, change the externalName in the service file by your OCI API domain.

```bash
export EXTERNAL_NAME=httpbin.org
envsubst < resources/1-service.yaml | kubectl apply -f -
```

#### 1.2 IngressRoute for External API

This is the ingress route for your service

```bash
kubectl apply -f resources/2-route.yaml
```

#### 1.3 Middleware to Strip Prefix

This is the middleware to strip the path prefix from the API.

```bash
kubectl apply -f resources/3-middleware-stripprefix.yaml
```

#### 1.4 Middleware to Secure the API using JWT

To secure your APIs with JWT authentication using OCI as an identity provider, you need to use your `jwks_uri`, which is found in your IdP's `/.well-known/openid-configuration` endpoint, for example:

```
https://idcs-5ba32fa3496f48289532f8fc10f47032.identity.oraclecloud.com/admin/v1/SigningCert/jwk
```

```bash
kubectl apply -f resources/4-middleware-jwt.yaml
```

#### 1.5 More Middlewares to Test

You can use more middlewares to handle your API traffic like for example:

- Distributed Rate Limiting or Standard Rate Limiting
- Open Policy Agents
- Or others

---

## 2. Test Your API

Test your API with the following curl request:

```bash
curl --location -k 'https://<External_IP>/teamoci3/ic/api/integration/v2/flows/rest/project/ORCL-R-REST_ECHO_REQUEST/ORCL-R-SAMPLE_ECHO/1.0/hello' \
--header 'Accept: application/json' \
--header 'Authorization: Bearer <<ADD YOUR TOKEN>>'
```

---

## Summary

- Enable external API routing by configuring Traefik with the required Helm flags.
- Deploy the provided manifests to expose and secure your external APIs via Traefik on OKE.
- Configure authentication using OCI's OIDC integration for robust security.

For further details on the base setup, refer to the [Traefik Hub documentation for OCI](https://doc.traefik.io/traefik-hub/operations/oracle-oci/oci-apim-marketplace).