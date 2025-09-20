# CawgIdentityDto

Configuration to add a CAWG identity assertion during signing. Presence of this object enables CAWG.


## Fields

| Field                                                           | Type                                                            | Required                                                        | Description                                                     |
| --------------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------- | --------------------------------------------------------------- |
| `Signer`                                                        | [*components.Signer](../../models/components/signer.md)         | :heavy_minus_sign:                                              | N/A                                                             |
| `SigningAlg`                                                    | [*components.SigningAlg](../../models/components/signingalg.md) | :heavy_minus_sign:                                              | Algorithm used for the CAWG identity signature.                 |
| `ReferencedAssertions`                                          | []*string*                                                      | :heavy_minus_sign:                                              | Assertion labels that the identity assertion should reference.  |
| `Timestamper`                                                   | **string*                                                       | :heavy_minus_sign:                                              | Timestamper to use ("digicert" or "custom:<url>").              |