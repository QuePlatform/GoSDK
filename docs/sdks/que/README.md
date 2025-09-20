# Que SDK

## Overview

Que API: Welcome to the Que Public HTTP API for C2PA (Content Authenticity Initiative) provenance management.

Our platform provides robust tools for working with digital asset provenance through C2PA manifests, enabling you to sign and verify digital assets to ensure their authenticity, origin, and processing history.

**Key Features:**
*   **Memory-Efficient Streaming**: Assets are processed using streaming techniques to minimize memory usage, supporting large files efficiently
*   **Verify**: Inspect and validate C2PA manifests embedded in assets with multiple detail levels
*   **Sign**: Embed comprehensive C2PA manifests into your assets with server-side cryptographic signatures
*   **Trust Management**: Retrieve and validate against current trust lists containing trusted certificate authorities and manufacturers
*   **Secure Uploads**: Direct-to-S3 uploads via presigned URLs for large assets

**Authentication:**
All endpoints (except for `/healthz`) are secured and require an API key to be passed in the `x-api-key` header.

**Processing Architecture:**
Assets are streamed from S3 or URLs to temporary storage during processing to ensure O(chunk_size) memory usage instead of O(file_size), enabling efficient handling of large files on containerized platforms.

Usage of this API is tracked via Firehose for billing and monitoring purposes.


Find more detailed documentation and tutorials here.
<https://docs.addque.org>

### Available Operations

* [VerifyAsset](#verifyasset) - Verify the C2PA manifest of an asset
* [SignAsset](#signasset) - Sign an asset with a C2PA manifest

## VerifyAsset

Analyzes a digital asset to find, validate, and report on any embedded C2PA manifests. This allows you to confirm the asset's provenance, authenticity, and processing history.

The asset is processed using memory-efficient streaming to temporary storage during verification. Returns detailed validation results including trust status, signer information, and any validation failures.


### Example Usage

<!-- UsageSnippet language="go" operationID="verifyAsset" method="post" path="/v1/verify" -->
```go
package main

import(
	"context"
	"os"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
)

func main() {
    ctx := context.Background()

    s := que.New(
        que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
    )

    res, err := s.VerifyAsset(ctx, components.VerifyRequest{
        Asset: components.CreateAssetRefDtoS3(
            components.S3{
                Bucket: "que-assets-dev",
                Key: "uploads/photo.jpg",
            },
        ),
        IncludeCertificates: que.Pointer(true),
    })
    if err != nil {
        log.Fatal(err)
    }
    if res.VerifyResponse != nil {
        // handle response
    }
}
```

### Parameters

| Parameter                                                            | Type                                                                 | Required                                                             | Description                                                          |
| -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `ctx`                                                                | [context.Context](https://pkg.go.dev/context#Context)                | :heavy_check_mark:                                                   | The context to use for the request.                                  |
| `request`                                                            | [components.VerifyRequest](../../models/components/verifyrequest.md) | :heavy_check_mark:                                                   | The request object to use for the request.                           |
| `opts`                                                               | [][operations.Option](../../models/operations/option.md)             | :heavy_minus_sign:                                                   | The options for this request.                                        |

### Response

**[*operations.VerifyAssetResponse](../../models/operations/verifyassetresponse.md), error**

### Errors

| Error Type                     | Status Code                    | Content Type                   |
| ------------------------------ | ------------------------------ | ------------------------------ |
| apierrors.ProblemResponseError | 400, 401, 403, 422             | application/problem+json       |
| apierrors.ProblemResponseError | 429                            | application/problem+json       |
| apierrors.ProblemResponseError | 500                            | application/problem+json       |
| apierrors.APIError             | 4XX, 5XX                       | \*/\*                          |

## SignAsset

Embeds a C2PA manifest into a digital asset and signs it using a server-side cryptographic key. The asset is processed using memory-efficient streaming to temporary storage before signing.

This operation cryptographically links the asset to its provenance information, creating an immutable record of the asset's origin, authorship, and any processing history.


### Example Usage

<!-- UsageSnippet language="go" operationID="signAsset" method="post" path="/v1/sign" -->
```go
package main

import(
	"context"
	"os"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
)

func main() {
    ctx := context.Background()

    s := que.New(
        que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
    )

    res, err := s.SignAsset(ctx, components.SignRequest{
        Asset: components.CreateAssetRefDtoS3(
            components.S3{
                Bucket: "que-assets-dev",
                Key: "uploads/photo.jpg",
            },
        ),
        Mode: components.ModeServerMeasure,
        ManifestJSON: que.Pointer("{\"title\":\"Original Photograph\",\"assertions\":[{\"label\":\"stds.schema-org.CreativeWork\",\"data\":{\"@context\":\"https://schema.org\",\"@type\":\"CreativeWork\",\"author\":[{\"@type\":\"Person\",\"name\":\"Jane Photographer\"}]}}]}"),
    })
    if err != nil {
        log.Fatal(err)
    }
    if res.SignResponse != nil {
        // handle response
    }
}
```

### Parameters

| Parameter                                                        | Type                                                             | Required                                                         | Description                                                      |
| ---------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- | ---------------------------------------------------------------- |
| `ctx`                                                            | [context.Context](https://pkg.go.dev/context#Context)            | :heavy_check_mark:                                               | The context to use for the request.                              |
| `request`                                                        | [components.SignRequest](../../models/components/signrequest.md) | :heavy_check_mark:                                               | The request object to use for the request.                       |
| `opts`                                                           | [][operations.Option](../../models/operations/option.md)         | :heavy_minus_sign:                                               | The options for this request.                                    |

### Response

**[*operations.SignAssetResponse](../../models/operations/signassetresponse.md), error**

### Errors

| Error Type                     | Status Code                    | Content Type                   |
| ------------------------------ | ------------------------------ | ------------------------------ |
| apierrors.ProblemResponseError | 400, 401, 403, 422             | application/problem+json       |
| apierrors.ProblemResponseError | 429                            | application/problem+json       |
| apierrors.ProblemResponseError | 500                            | application/problem+json       |
| apierrors.APIError             | 4XX, 5XX                       | \*/\*                          |