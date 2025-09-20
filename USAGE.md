<!-- Start SDK Example Usage [usage] -->
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