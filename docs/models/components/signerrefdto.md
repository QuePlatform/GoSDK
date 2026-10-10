# SignerRefDto

Reference to credentials for signing.


## Supported Types

### SignerRefDtoEnv

```go
signerRefDto := components.CreateSignerRefDtoSignerRefDtoEnv(components.SignerRefDtoEnv{/* values here */})
```

### SignerRefDtoLocal

```go
signerRefDto := components.CreateSignerRefDtoSignerRefDtoLocal(components.SignerRefDtoLocal{/* values here */})
```

## Union Discrimination

Use the `Type` field to determine which variant is active, then access the corresponding field:

```go
switch signerRefDto.Type {
	case components.SignerRefDtoTypeSignerRefDtoEnv:
		// signerRefDto.SignerRefDtoEnv is populated
	case components.SignerRefDtoTypeSignerRefDtoLocal:
		// signerRefDto.SignerRefDtoLocal is populated
}
```
