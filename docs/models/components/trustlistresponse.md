# TrustListResponse

The current C2PA trust list containing trusted certificate authorities, hardware manufacturers, and trust policies used for manifest verification.


## Fields

| Field                                                     | Type                                                      | Required                                                  | Description                                               | Example                                                   |
| --------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| `Version`                                                 | *string*                                                  | :heavy_check_mark:                                        | Version identifier for this trust list bundle.            | dev-1                                                     |
| `IssuedAt`                                                | [time.Time](https://pkg.go.dev/time#Time)                 | :heavy_check_mark:                                        | Timestamp when this trust list was issued.                | 2024-01-15T10:00:00Z                                      |
| `Data`                                                    | [components.Data](../../models/components/data.md)        | :heavy_check_mark:                                        | The trust list data containing certificates and policies. |                                                           |