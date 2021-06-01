---
layout: post
title: Scaleway Object Storage using Java
comments: true
tags:
- Scaleway
- Object Storage
- Java
- S3
---

Until recently, I used Google Cloud Storage to periodically store and retrieve 
a few TB of data. Although Google services and their premium network 
solution are by far the best solution I worked with (the bandwidth and latency 
are just incredible from anywhere to anywhere in the World), the price is too
expensive for my use case. 

Looking at affordable solutions, and after reviewing a dozen solutions going 
from Digital Ocean, Hetzner, OVH, and more, I decided to go with Scaleway.

Why Scaleway? their object storage solution is cheap, you have great 
configuration options to manage privacy, lifecycle rules, and up to 100Gbp/s 
bandwidth. Furthermore, despite their offer uses the public Internet, the 
peering is quite good Worldwide, meaning you can expect a good bandwidth even if
you download or upload from Australia or the US.

However, nothing is perfect. The Scaleway documentation is quite minimal. There
are no official client libraries, nor examples of basic operations
like downloading an object from a Scaleway bucket in Java. That's the purpose of
this blog post.

Scaleway mimics the Amazon S3 interface and the Amazon SDK is configurable 
enough to be used with Scaleway. To use Scaleway Object Storage in Java, you
need to first import the Amazon SDK as a dependency.

Here is the dependency to use if you are using Gradle:

```groovy
implementation 'software.amazon.awssdk:s3:2.16.74'
```

Did you notice Amazon owns the domain _amazon.software_? yes, _software_ is a 
top-level domain.

The next step is to create a client instance:

```java
S3Client.builder()
    .region(Region.of("fr-par"))
    .endpointOverride(URI.create("https://s3.fr-par.scw.cloud"))
    .credentialsProvider(
        StaticCredentialsProvider.create(
            AwsBasicCredentials.create(accessKey, secretKey))
    ).build();
```

In the code snippet above, _accessKey_ and _secretKey_ are your credentials to
get from the Scaleway dashboard.

Once you have a client instance, it is really easy to interact with the Object
Storage service. For instance, here is an example to retrieve an object file as 
an _InputStream_:

```java
final GetObjectRequest objectRequest =
    GetObjectRequest
        .builder()
        .key("path/to/your/object/file")
        .bucket("your-scaleway-bucket")
        .build();

client.getObject();
```

Here is a link to Gist for a basic class example:

[https://gist.github.com/lpellegr/e2a05f24181c33a65467fbc189a2e115](https://gist.github.com/lpellegr/e2a05f24181c33a65467fbc189a2e115)
