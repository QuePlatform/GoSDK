# AssetRefDto

A reference to a digital asset, either stored in S3 or accessible via URL. Files are streamed efficiently to temporary storage during processing to minimize memory usage.


## Supported Types

### S3

```go
assetRefDto := components.CreateAssetRefDtoS3(components.S3{/* values here */})
```

### PresignedURL

```go
assetRefDto := components.CreateAssetRefDtoPresignedURL(components.PresignedURL{/* values here */})
```

## Union Discrimination

Use the `Type` field to determine which variant is active, then access the corresponding field:

```go
switch assetRefDto.Type {
	case components.AssetRefDtoTypeS3:
		// assetRefDto.S3 is populated
	case components.AssetRefDtoTypePresignedURL:
		// assetRefDto.PresignedURL is populated
}
```
