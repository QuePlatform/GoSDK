# Utility
(*Utility*)

## Overview

Service-level endpoints for health checks and configuration.

### Available Operations

* [GetHealthCheck](#gethealthcheck) - Service Health Check
* [GetTrustList](#gettrustlist) - Retrieve the current C2PA trust bundle

## GetHealthCheck

Performs a basic health check of the API service. This endpoint does not require authentication and is typically used by monitoring systems.

### Example Usage

<!-- UsageSnippet language="go" operationID="getHealthCheck" method="get" path="/healthz" -->
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

    res, err := s.Utility.GetHealthCheck(ctx)
    if err != nil {
        log.Fatal(err)
    }
    if res.HealthzResponse != nil {
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

**[*operations.GetHealthCheckResponse](../../models/operations/gethealthcheckresponse.md), error**

### Errors

| Error Type         | Status Code        | Content Type       |
| ------------------ | ------------------ | ------------------ |
| apierrors.APIError | 4XX, 5XX           | \*/\*              |

## GetTrustList

Fetches the latest C2PA trust list containing trusted certificate authorities, hardware manufacturers, and trust policies.

The trust list is used during manifest verification to:
- Validate signer certificates against trusted Certificate Authorities
- Verify hardware manufacturer claims for camera-captured content
- Apply trust policies for different validation scenarios

Trust lists are versioned and should be refreshed periodically as new trusted entities are added or certificates expire.


### Example Usage

<!-- UsageSnippet language="go" operationID="getTrustList" method="get" path="/v1/trust-list" -->
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

    res, err := s.Utility.GetTrustList(ctx)
    if err != nil {
        log.Fatal(err)
    }
    if res.TrustListResponse != nil {
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

**[*operations.GetTrustListResponse](../../models/operations/gettrustlistresponse.md), error**

### Errors

| Error Type                     | Status Code                    | Content Type                   |
| ------------------------------ | ------------------------------ | ------------------------------ |
| apierrors.ProblemResponseError | 401, 403                       | application/problem+json       |
| apierrors.ProblemResponseError | 429                            | application/problem+json       |
| apierrors.ProblemResponseError | 500                            | application/problem+json       |
| apierrors.APIError             | 4XX, 5XX                       | \*/\*                          |