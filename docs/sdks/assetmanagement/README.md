# AssetManagement
(*AssetManagement*)

## Overview

Helper endpoints for handling asset uploads.

### Available Operations

* [GetPresignedURL](#getpresignedurl) - Get an S3 presigned URL for secure uploads

## GetPresignedURL

Generates a temporary, cryptographically signed URL that allows secure direct upload of assets to S3 without exposing AWS credentials.

This is the recommended approach for uploading assets, especially large files, as it:
- Avoids sending large payloads through the API server
- Provides secure, time-limited access to S3
- Enables resumable uploads for better reliability
- Reduces API server memory usage and network overhead

Use the returned URL to make a PUT request with your asset file, then use the returned key for subsequent sign/verify operations.


### Example Usage

<!-- UsageSnippet language="go" operationID="getPresignedUrl" method="post" path="/v1/assets/presign" -->
```go
package main

import(
	"context"
	"os"
	que "github.com/QuePlatform/GoSDK"
	"log"
)

func main() {
    ctx := context.Background()

    s := que.New(
        que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
    )

    res, err := s.AssetManagement.GetPresignedURL(ctx)
    if err != nil {
        log.Fatal(err)
    }
    if res.PresignResponse != nil {
        // handle response
    }
}
```

### Parameters

| Parameter                                                | Type                                                     | Required                                                 | Description                                              |
| -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- | -------------------------------------------------------- |
| `ctx`                                                    | [context.Context](https://pkg.go.dev/context#Context)    | :heavy_check_mark:                                       | The context to use for the request.                      |
| `opts`                                                   | [][operations.Option](../../models/operations/option.md) | :heavy_minus_sign:                                       | The options for this request.                            |

### Response

**[*operations.GetPresignedURLResponse](../../models/operations/getpresignedurlresponse.md), error**

### Errors

| Error Type                     | Status Code                    | Content Type                   |
| ------------------------------ | ------------------------------ | ------------------------------ |
| apierrors.ProblemResponseError | 401, 403                       | application/problem+json       |
| apierrors.ProblemResponseError | 429                            | application/problem+json       |
| apierrors.ProblemResponseError | 500                            | application/problem+json       |
| apierrors.APIError             | 4XX, 5XX                       | \*/\*                          |