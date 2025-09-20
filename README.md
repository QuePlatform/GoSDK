# que

Developer-friendly & type-safe Go SDK specifically catered to leverage *que* API.

<div align="left">
    <a href="https://www.speakeasy.com/?utm_source=que&utm_campaign=go"><img src="https://www.speakeasy.com/assets/badges/built-by-speakeasy.svg" /></a>
    <a href="https://opensource.org/licenses/MIT">
        <img src="https://img.shields.io/badge/License-MIT-blue.svg" style="width: 100px; height: 28px;" />
    </a>
</div>


<br /><br />
> [!IMPORTANT]
> This SDK is not yet ready for production use. To complete setup please follow the steps outlined in your [workspace](https://app.speakeasy.com/org/que-7x8/que). Delete this section before > publishing to a package manager.

<!-- Start Summary [summary] -->
## Summary

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


For more information about the API: [Find more detailed documentation and tutorials here.](https://docs.addque.org)
<!-- End Summary [summary] -->

<!-- Start Table of Contents [toc] -->
## Table of Contents
<!-- $toc-max-depth=2 -->
* [que](#que)
  * [SDK Installation](#sdk-installation)
  * [SDK Example Usage](#sdk-example-usage)
  * [Authentication](#authentication)
  * [Available Resources and Operations](#available-resources-and-operations)
  * [Retries](#retries)
  * [Error Handling](#error-handling)
  * [Server Selection](#server-selection)
  * [Custom HTTP Client](#custom-http-client)
* [Development](#development)
  * [Maturity](#maturity)
  * [Contributions](#contributions)

<!-- End Table of Contents [toc] -->

<!-- Start SDK Installation [installation] -->
## SDK Installation

To add the SDK as a dependency to your project:
```bash
go get github.com/QuePlatform/GoSDK
```
<!-- End SDK Installation [installation] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

### Example

```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
	"os"
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
				Key:    "uploads/photo.jpg",
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
<!-- End SDK Example Usage [usage] -->

<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security scheme globally:

| Name         | Type   | Scheme  | Environment Variable |
| ------------ | ------ | ------- | -------------------- |
| `APIKeyAuth` | apiKey | API key | `QUE_API_KEY_AUTH`   |

You can configure it using the `WithSecurity` option when initializing the SDK client instance. For example:
```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
	"os"
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
				Key:    "uploads/photo.jpg",
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
<!-- End Authentication [security] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

<details open>
<summary>Available methods</summary>

### [AssetManagement](docs/sdks/assetmanagement/README.md)

* [GetPresignedURL](docs/sdks/assetmanagement/README.md#getpresignedurl) - Get an S3 presigned URL for secure uploads

### [Que SDK](docs/sdks/que/README.md)

* [VerifyAsset](docs/sdks/que/README.md#verifyasset) - Verify the C2PA manifest of an asset
* [SignAsset](docs/sdks/que/README.md#signasset) - Sign an asset with a C2PA manifest

### [Utility](docs/sdks/utility/README.md)

* [GetHealthCheck](docs/sdks/utility/README.md#gethealthcheck) - Service Health Check
* [GetTrustList](docs/sdks/utility/README.md#gettrustlist) - Retrieve the current C2PA trust bundle

</details>
<!-- End Available Resources and Operations [operations] -->

<!-- Start Retries [retries] -->
## Retries

Some of the endpoints in this SDK support retries. If you use the SDK without any configuration, it will fall back to the default retry strategy provided by the API. However, the default retry strategy can be overridden on a per-operation basis, or across the entire SDK.

To change the default retry strategy for a single API call, simply provide a `retry.Config` object to the call by using the `WithRetries` option:
```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"github.com/QuePlatform/GoSDK/retry"
	"log"
	"models/operations"
	"os"
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
				Key:    "uploads/photo.jpg",
			},
		),
		IncludeCertificates: que.Pointer(true),
	}, operations.WithRetries(
		retry.Config{
			Strategy: "backoff",
			Backoff: &retry.BackoffStrategy{
				InitialInterval: 1,
				MaxInterval:     50,
				Exponent:        1.1,
				MaxElapsedTime:  100,
			},
			RetryConnectionErrors: false,
		}))
	if err != nil {
		log.Fatal(err)
	}
	if res.VerifyResponse != nil {
		// handle response
	}
}

```

If you'd like to override the default retry strategy for all operations that support retries, you can use the `WithRetryConfig` option at SDK initialization:
```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"github.com/QuePlatform/GoSDK/retry"
	"log"
	"os"
)

func main() {
	ctx := context.Background()

	s := que.New(
		que.WithRetryConfig(
			retry.Config{
				Strategy: "backoff",
				Backoff: &retry.BackoffStrategy{
					InitialInterval: 1,
					MaxInterval:     50,
					Exponent:        1.1,
					MaxElapsedTime:  100,
				},
				RetryConnectionErrors: false,
			}),
		que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
	)

	res, err := s.VerifyAsset(ctx, components.VerifyRequest{
		Asset: components.CreateAssetRefDtoS3(
			components.S3{
				Bucket: "que-assets-dev",
				Key:    "uploads/photo.jpg",
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
<!-- End Retries [retries] -->

<!-- Start Error Handling [errors] -->
## Error Handling

Handling errors in this SDK should largely match your expectations. All operations return a response object or an error, they will never return both.

By Default, an API error will return `apierrors.APIError`. When custom error responses are specified for an operation, the SDK may also return their associated error. You can refer to respective *Errors* tables in SDK docs for more details on possible error types for each operation.

For example, the `VerifyAsset` function may return the following errors:

| Error Type                     | Status Code        | Content Type             |
| ------------------------------ | ------------------ | ------------------------ |
| apierrors.ProblemResponseError | 400, 401, 403, 422 | application/problem+json |
| apierrors.ProblemResponseError | 429                | application/problem+json |
| apierrors.ProblemResponseError | 500                | application/problem+json |
| apierrors.APIError             | 4XX, 5XX           | \*/\*                    |

### Example

```go
package main

import (
	"context"
	"errors"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/apierrors"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
	"os"
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
				Key:    "uploads/photo.jpg",
			},
		),
		IncludeCertificates: que.Pointer(true),
	})
	if err != nil {

		var e *apierrors.ProblemResponseError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}

		var e *apierrors.ProblemResponseError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}

		var e *apierrors.ProblemResponseError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}

		var e *apierrors.APIError
		if errors.As(err, &e) {
			// handle error
			log.Fatal(e.Error())
		}
	}
}

```
<!-- End Error Handling [errors] -->

<!-- Start Server Selection [server] -->
## Server Selection

### Server Variables

The default server `https://{environment}.addque.org/` contains variables and is set to `https://dev-api.addque.org/` by default. To override default values, the following options are available when initializing the SDK client instance:

| Variable      | Option                                | Default     | Description                 |
| ------------- | ------------------------------------- | ----------- | --------------------------- |
| `environment` | `WithEnvironment(environment string)` | `"dev-api"` | The deployment environment. |

#### Example

```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
	"os"
)

func main() {
	ctx := context.Background()

	s := que.New(
		que.WithEnvironment("<value>"),
		que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
	)

	res, err := s.VerifyAsset(ctx, components.VerifyRequest{
		Asset: components.CreateAssetRefDtoS3(
			components.S3{
				Bucket: "que-assets-dev",
				Key:    "uploads/photo.jpg",
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

### Override Server URL Per-Client

The default server can be overridden globally using the `WithServerURL(serverURL string)` option when initializing the SDK client instance. For example:
```go
package main

import (
	"context"
	que "github.com/QuePlatform/GoSDK"
	"github.com/QuePlatform/GoSDK/models/components"
	"log"
	"os"
)

func main() {
	ctx := context.Background()

	s := que.New(
		que.WithServerURL("https://dev-api.addque.org/"),
		que.WithSecurity(os.Getenv("QUE_API_KEY_AUTH")),
	)

	res, err := s.VerifyAsset(ctx, components.VerifyRequest{
		Asset: components.CreateAssetRefDtoS3(
			components.S3{
				Bucket: "que-assets-dev",
				Key:    "uploads/photo.jpg",
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
<!-- End Server Selection [server] -->

<!-- Start Custom HTTP Client [http-client] -->
## Custom HTTP Client

The Go SDK makes API calls that wrap an internal HTTP client. The requirements for the HTTP client are very simple. It must match this interface:

```go
type HTTPClient interface {
	Do(req *http.Request) (*http.Response, error)
}
```

The built-in `net/http` client satisfies this interface and a default client based on the built-in is provided by default. To replace this default with a client of your own, you can implement this interface yourself or provide your own client configured as desired. Here's a simple example, which adds a client with a 30 second timeout.

```go
import (
	"net/http"
	"time"

	"github.com/QuePlatform/GoSDK"
)

var (
	httpClient = &http.Client{Timeout: 30 * time.Second}
	sdkClient  = que.New(que.WithClient(httpClient))
)
```

This can be a convenient way to configure timeouts, cookies, proxies, custom headers, and other low-level configuration.
<!-- End Custom HTTP Client [http-client] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->

# Development

## Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

## Contributions

While we value open-source contributions to this SDK, this library is generated programmatically. Any manual changes added to internal files will be overwritten on the next generation. 
We look forward to hearing your feedback. Feel free to open a PR or an issue with a proof of concept and we'll do our best to include it in a future release. 

### SDK Created by [Speakeasy](https://www.speakeasy.com/?utm_source=que&utm_campaign=go)
