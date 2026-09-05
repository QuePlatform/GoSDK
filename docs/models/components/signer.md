# Signer


## Supported Types

### SignerUseMainSigner

```go
signer := components.CreateSignerSignerUseMainSigner(components.SignerUseMainSigner{/* values here */})
```

### SignerSeparate

```go
signer := components.CreateSignerSignerSeparate(components.SignerSeparate{/* values here */})
```

## Union Discrimination

Use the `Type` field to determine which variant is active, then access the corresponding field:

```go
switch signer.Type {
	case components.SignerTypeSignerUseMainSigner:
		// signer.SignerUseMainSigner is populated
	case components.SignerTypeSignerSeparate:
		// signer.SignerSeparate is populated
}
```
