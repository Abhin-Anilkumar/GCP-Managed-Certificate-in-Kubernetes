# Nginx Deployment on GKE with Managed SSL Certificates

This repository contains Kubernetes configuration files for deploying an Nginx server on Google Kubernetes Engine (GKE), utilizing Google-managed SSL certificates for secure HTTPS connections, and enforcing HTTPS with a FrontendConfig for HTTP to HTTPS redirection.

## Overview

The deployment consists of:
- An Nginx Deployment with 3 replicas, serving content on port 80.
- A LoadBalancer Service to expose the Nginx Deployment externally.
- A ManagedCertificate resource for handling SSL certificates for the domain `www.example.com`.
- A FrontendConfig to enforce HTTPS redirection.
- An Ingress to manage external access with an SSL certificate and HTTPS redirection enabled.

## Prerequisites

- A Google Cloud account and a GKE cluster.
- The `gcloud` CLI and `kubectl` CLI installed and configured to communicate with your GKE cluster.
- A domain name, with DNS configured to point to the external IP address assigned by the Ingress.

## Deployment Steps

1. **Enable Google-managed certificates on your GKE cluster** (if not already enabled). This feature is required for the ManagedCertificate resource to work.

2. **Apply the Kubernetes configuration:**

   Deploy the resources to your GKE cluster by running:

   ```sh
   kubectl apply -f deployment.yaml
   kubectl apply -f certificate.yaml
   kubectl apply -f ingress.yaml

3. **Verify the deployment:**

   Check the status of your deployment, service, and     ingress to ensure everything is running as expected:

   ```sh
   kubectl get deployment,service,Ingress

## **DNS Configuration:**

Ensure your domain's DNS settings are configured to point to the external IP address provided by the Ingress.

## **Access Your Application:**

Once the DNS changes have propagated, you should be able to access your Nginx server via https://www.example.com with a secure connection.

## **Important Notes**

It may take a few minutes for the SSL certificate to be provisioned and for the HTTPS redirection to become active.
The ManagedCertificate and FrontendConfig resources are specific to GKE and may not be available in other Kubernetes environments.


## **Troubleshooting**

If the ManagedCertificate is not being issued, verify that the DNS configuration for your domain is correct and that the Google-managed certificates feature is enabled on your cluster.
For any issues with the FrontendConfig not redirecting HTTP to HTTPS, ensure the resource is correctly defined and associated with your Ingress.
