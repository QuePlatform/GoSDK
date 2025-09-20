# Evidence

Cryptographic evidence about the signature that was applied.


## Fields

| Field                                            | Type                                             | Required                                         | Description                                      | Example                                          |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| `Signer`                                         | *string*                                         | :heavy_check_mark:                               | Identifier for the signing entity/key used.      | env_dev                                          |
| `Alg`                                            | [components.Alg](../../models/components/alg.md) | :heavy_check_mark:                               | The cryptographic algorithm used for signing.    | ES256                                            |