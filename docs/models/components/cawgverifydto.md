# CawgVerifyDto

Options controlling CAWG identity validation behavior during verification.


## Fields

| Field                                                             | Type                                                              | Required                                                          | Description                                                       |
| ----------------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------- |
| `Validate`                                                        | **bool*                                                           | :heavy_minus_sign:                                                | Whether to run CAWG identity validation.                          |
| `RequireValidIdentity`                                            | **bool*                                                           | :heavy_minus_sign:                                                | Whether to fail verification if CAWG identity is missing/invalid. |