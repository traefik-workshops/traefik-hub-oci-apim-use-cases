# Traefik OCI Integration Repository

This repository provides comprehensive guides and configurations for integrating Traefik with Oracle Cloud Infrastructure (OCI) and Oracle Kubernetes Engine (OKE). It offers two distinct deployment approaches to suit different use cases and requirements.

> This repository extends the official Traefik Hub documentation:  
> https://doc.traefik.io/traefik-hub/operations/oracle-oci/oci-apim-marketplace

---

## Deployment Options

### 1. Traefik API Management (`traefik_apim/`)

**Full-featured API Management solution with Developer Portal**

This deployment provides a complete API management platform with the following features:

- **API Management**: Full lifecycle API management capabilities
- **Developer Portal**: Self-service portal for developers to discover and consume APIs
- **API Plans**: Bronze and Gold tier plans with rate limiting and quotas
- **Catalog Items**: API catalog with subscription management
- **Single Sign-On (SSO)**: Integration with OCI Identity Cloud Service
- **JWT Authentication**: Secure API access with JWT tokens
- **External API Support**: Expose APIs hosted outside your Kubernetes cluster

**Use this option when you need:**
- Complete API management capabilities
- Developer portal for API consumers
- API monetization and tiered access plans
- Comprehensive authentication and authorization

**Key Components:**
- ExternalName Service for API exposure
- IngressRoute with API annotation for Traefik Hub
- Middleware for path stripping and JWT security
- API resource definitions
- API Plans (Gold and Bronze)
- Catalog Items for API discovery
- Developer Portal configuration

### 2. Traefik API Gateway (`traefik_apigateway/`)

**Lightweight API Gateway for direct API exposure**

This deployment provides a streamlined API gateway solution focusing on:

- **API Gateway**: Direct API routing and exposure
- **JWT Authentication**: Secure API access with JWT tokens
- **External API Support**: Expose APIs hosted outside your Kubernetes cluster
- **Middleware Support**: Rate limiting, OPA policies, and other traffic management

**Use this option when you need:**
- Simple API gateway functionality
- Direct API exposure without management overhead
- JWT-based security
- Lightweight deployment with minimal components

**Key Components:**
- ExternalName Service for API exposure
- IngressRoute for direct API routing
- Middleware for path stripping
- JWT authentication middleware
- Support for additional middleware (rate limiting, OPA, etc.)

---

## Quick Start

### Prerequisites

- Oracle Kubernetes Engine (OKE) cluster
- Traefik installed via Helm
- Access to external OCI APIs
- OCI Identity Cloud Service configuration

### Choose Your Deployment

1. **For Full API Management**: Navigate to [`traefik_apim/`](./traefik_apim/README.md)
2. **For API Gateway Only**: Navigate to [`traefik_apigateway/`](./traefik_apigateway/README.md)


## Getting Started

1. **Choose your deployment approach** based on your requirements
2. **Follow the specific README** in the corresponding directory
3. **Deploy the resources** using the provided YAML files
4. **Configure authentication** with your OCI Identity Cloud Service
5. **Test your APIs** using the provided examples

---

## Support and Documentation

- **Traefik Hub Documentation**: https://doc.traefik.io/traefik-hub/
- **OCI Integration Guide**: https://doc.traefik.io/traefik-hub/operations/oracle-oci/oci-apim-marketplace
- **External API Guide**: https://doc.traefik.io/traefik-hub/api-management/external-api

---

## Repository Structure

```
OCI_APIM_repo/
├── README.md                    # This file
├── traefik_apim/               # Full API Management deployment
│   ├── README.md               # Detailed setup instructions
│   ├── resources/              # Kubernetes manifests
│   └── img/                    # Documentation images
└── traefik_apigateway/         # API Gateway deployment
    ├── README.md               # Detailed setup instructions
    └── resources/              # Kubernetes manifests
```