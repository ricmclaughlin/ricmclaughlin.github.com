---
layout: post
title: "AWS - Certificate Manager"
description: ""
category: posts
tags: [aws, aws-services, security-id-compliance, aws-solutions-arch-pro, aws-services]
---
{% include JB/setup %}

[AWS Certificate Manager](https://aws.amazon.com/certificate-manager/)

AWS Certificate Manager (ACM) to provision, manages, and deploys public and private SSL/TLS certificates for use with AWS services and internal connected resources. ACM removes the time-consuming manual process of purchasing, uploading, and renewing SSL/TLS certificates.

ACM works with Load Balancers, CloudFront, &amp; API Gateway. Public Certificates must be issued by a trusted Cert Authority (CA) and validate domain ownership using email or DNS. There is the ability to set up creating a private CA. ACM is regional; certs can not be copied across regions.

Server Name Indication (SNI) enables loading multiple SSL certs on a single server then the client indicates which hostname to connect to. ACM supports SNI.

