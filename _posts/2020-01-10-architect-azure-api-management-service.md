---
author: Olcay Bayram
layout: post
categories: Azure
published: true
title: Architect Azure API Management service
subtitle: Architect serverless solutions in Azure
image: /img/azure-api-gateway-policy-wizard.png
tags:
  - cloud
  - azure
  - gateway
  - api
  - cache
  - security
serieName: serverless
---

Let's setup an API gateway using the Azure API Management service with a nice architecture.

An API gateway is positioned between your APIs and the Internet. You can control how the APIs are exposed or limit the usage per subscription through the Azure portal.

## Why would I use the Azure API Management service?

It is a native Azure SaaS (software as a service) which brings nice pros;
- API documentation
- Rate limiting access
- Health monitoring
- Modern formats like JSON
- Connections to any API
- Analytics
- Security
- Built-in caching
- Network tracing

## Is there any trade-off?

Yes, [the circuit breaker policy](https://feedback.azure.com/forums/248703-api-management/suggestions/15527100-circuit-breaker-policy) is not implemented yet. You can build your own API gateway with a circuit breaker using third-party libraries like [Ocelot](https://github.com/ThreeMammals/Ocelot) and [Polly](https://github.com/App-vNext/Polly) with [Quality of Service](https://ocelot.readthedocs.io/en/latest/features/qualityofservice.html) configured but does it worth to go down to a PaaS (platform as a service) instead of a SaaS?

<!--more-->

## Setup an API Management service

1. Sign into the [Azure portal](https://portal.azure.com/#create/hub).
1. Create a resource through __Integration__, and then __API Management__.
1. Give a globally unique name to your resource.
1. Select __Consumption (99.9SLA, %)__ as the pricing tier because this serverless plan is much faster to create for an ad-hoc testing.
1. Click __Create__.
1. Deployment will start and it may take several minutes.

When the deployment is complete you will get a notification email to the address you provided as an administrator. Once it is deployed we import our first API.

## Import an API

1. Sign into the [Azure portal](https://portal.azure.com/).
1. Go to __All Resources__, and then select your API gateway.
1. Under __API Management__, click __APIs__
1. Choose your specification. If you are using [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) library in your API that means you already have [OpenAPI specification](https://swagger.io/specification/).
1. On the __Create from OpenAPI specification__ page, paste the swagger JSON URL of your API. The other fields will be populated according to your API.
1. Click __Create__.

Now, you can test the gateway through the __Test__ tab on the API details page.

# Subscriptions

A subscription is like authenticating a client with a public key which can be transferred through in the headers or in the query string. The subscription can be scoped to All APIs, a single API or a group of APIs (called a product).

The unique subscription keys can be regenerated at any time. Every subscription has two keys, a primary and a secondary to avoid downtime.

The default header name is `Ocp-Apim-Subscription-Key`, and the default query string is `subscription-key`. If the key is not passed in the header, or as a query string in the URL, you'll get a __401 Access Denied__ response from the API gateway.

# Policies

In Azure API Management, administrators can use policies to alter the behavior of APIs through configuration. Policies execute at four different times:

- __Inbound__: These policies execute when a request is received from a client.
- __Backend__: These policies execute before a request is forwarded to a managed API.
- __Outbound__: These policies execute before a response is sent to a client.
- __On-Error__: These policies execute when an exception is raised.

There is one more scope in addition to subscription scopes for policies and it is operation policy scope. We can determine order of the policies. If we place `<base />` tag before a policy that means higher policies will be applied first and vice versa.

## Commonly used policies

- Check HTTP header
- Limit call rate by subscription
- Restrict caller IP's
- Authenticate
- CORS
- JSONP
- Convert XML to JSON
- Rewrite URL

## Cache policy

By caching the compiled responses we can reduce processing time in our APIs and respond faster. We can add an outbound policy to cache the responses and an inbound policy to check if there is a cached response for the current request. You can see these two policies in the example below which is using a `vary-by-query-parameter` tag to store separate responses per id in a query string:

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false"
        downstream-caching-type="none"
        must-revalidate="true" caching-type="internal">
            <vary-by-query-parameter>id</vary-by-query-parameter>
        </cache-lookup>
    </inbound>
    <backend>
        <base />
    </backend>
    <outbound>
        <cache-store duration="60" />
        <base />
    </outbound>
    </on-error>
        <base />
    </on-error>
</policies>
```

The `vary-by-developer` attribute is separating responses according to the subscription key.

As you can already notice, the caching type for this example is internal. We can use an external caching service as well.

## Add an external cache

1. Create an Azure Cache for Redis resource.
1. Go to your API Management service and click __External cache__ under __Settings__ and then click __+ Add__.
1. Choose your Redis cache instance and a location to use from. The other fields will be populated.
1. Click __Save__.
1. Go to __APIs__ under __API Management__ an then select the API and click the pencil next to the __cache-lookup__ to edit the policy.
1. Change caching type from __Internal__ to __External__

You can test your API in the __Test__ tab to see if it is giving unchanged results.

## Remove unnecessary info from response header

ASP.NET add a `X-Powered-By: ASP.NET` header to our APIs by default but this could allow a malicious user to attempt to exploit any bugs known for the technology stack. We can remove this header from a response by adding `set-header` policy to outbound as in the example below;

```xml
<outbound>
   <set-header name="X-Powered-By" exists-action="delete" />
   <base />
</outbound>
```

## Replace content with a transformation policy

If we build an API with the [HATEOAS](https://restfulapi.net/hateoas/) constraint, that means we have links in the responses. Since the gateway is overriding the URLs, we may need to replace the links in the response body. We can do it by adding `<redirect-content-urls />` tag to the `<outbound>` element as below;

```xml
<outbound>
   <set-header name="X-Powered-By" exists-action="delete" />
   <redirect-content-urls />
   <base />
</outbound>
```

## Limit call rate by subscription

If we place the `rate-limit-by-key` tag inside the `inbound` element, it will limit the call rate by subscription. A subscription can call this API 10 times in 60 seconds and only the successful responses are counted.

```xml
<inbound>
    <rate-limit-by-key calls="10"
              renewal-period="60"
              increment-condition="@(context.Response.StatusCode == 200)"
              counter-key="@(context.Subscription.Id)"/>
    <base />
</inbound>
```

## Validate a certificate to allow requests

If we want to use certificate authentication in our API gateway, we can validate it by an inbound policy.

1. Under __Settings__, click __Custom domains__ in your API Management service.
1. Toggle __Yes__ for the __Request client certificate__ option, and then click __Save__.
1. Replace the `<inbound>` node of the policy file with the following XML, while replacing `desired-thumbprint` part with your certificate thumbprint;

```xml
<inbound>
    <choose>
        <when condition="@(context.Request.Certificate == null || 
        context.Request.Certificate.Thumbprint != "desired-thumbprint")" >
            <return-response>
                <set-status code="403" reason="Invalid client certificate" />
            </return-response>
        </when>
    </choose>
    <base />
</inbound>
```

Please, write the policies that you find useful in the comments.
