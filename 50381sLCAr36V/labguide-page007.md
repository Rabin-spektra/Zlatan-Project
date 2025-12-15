# Google Cloud NGFW Enterprise Lab

This lab demonstrates how to deploy and secure workloads using Google Cloud NGFW Enterprise, a native Google Cloud service powered by Palo Alto Networks’ Threat Prevention technologies.

Cloud NGFW Enterprise is a fully distributed firewall solution that delivers advanced protection for Google Cloud environments against both internal and external threats such as intrusion attempts, malware, spyware, and command‑and‑control activity. It operates by creating Google‑managed zonal firewall endpoints that leverage packet interception technology to transparently capture workload traffic and perform deep packet inspection.

## Pre-requisites

* Access to Google [Cloud Shell](https://cloud.google.com/shell/docs/how-cloud-shell-works), or a local machine with a Terraform or gcloud installation.
* A Google Cloud [project](https://cloud.google.com/resource-manager/docs/creating-managing-projects) to host the deployment.
* A Google Cloud [billing project](https://cloud.google.com/billing/docs/how-to/view-linked).

## Lab Architecture

In this architecture, a VPC network hosts two virtual machines: **client-vm** and **web-vm**. The **client-vm** is used to simulate threats targeting both north/south internet traffic and east/west traffic directed at the web application running on the web-vm.
To mitigate these threats, a Cloud NGFW endpoint is deployed and linked to the network. Firewall policies applied to this endpoint define how traffic is inspected and filtered by Cloud NGFW, ensuring protection against malicious activity.

![diagram.png](https://docs-api.cloudlabs.ai/repos/raw.githubusercontent.com/CloudLabs-AI/Lab-Guide/main/12551dPslnSJF/images/diagram.png)

## Prepare for Deployment

Enable the required APIs, retrieve the deployment files, and configure the environment variables.

> [!IMPORTANT]
> This tutorial assumes you are using Cloud Shell to provision all resources.

1. Open [Google Cloud Shell](https://shell.cloud.google.com) <img src="https://storage.googleapis.com/cloud-training/images/devshell.png" alt="cloudshell.png" /> and enable the required APIs.

   **Note:** Once you click on Cloud Shell, then click Authorize, and then enter the password.


2. Open [Google Cloud Shell](https://shell.cloud.google.com) <img src="https://storage.googleapis.com/cloud-training/images/devshell.png" alt="cloudshell.png" /> and enable the required APIs.

**Note:** Once you click on Cloud Shell, then click Authorize, and then enter the password.

    ```
    gcloud services enable compute.googleapis.com
    gcloud services enable networksecurity.googleapis.com
    gcloud services enable firewallinsights.googleapis.com
    ```

2. Set environment variables for your **deployment** *project* and billing project.

    export PROJECT_ID=*YOUR_PROJECT_ID*
    export BILLING_ID=**YOUR_BILLING_PROJECT_ID** <br>

3. Set environment variables for your organization ID, deployment region, zone, and naming prefix.

      ```
      export ORG_ID=$(gcloud projects describe $PROJECT_ID --format=json | jq -r '.parent.id')
      export REGION=us-central1
      export ZONE=us-central1-a
      export PREFIX=panw
      ```