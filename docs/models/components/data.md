# Data

The trust list data containing certificates and policies.


## Fields

| Field                                                                               | Type                                                                                | Required                                                                            | Description                                                                         |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `Manufacturers`                                                                     | []*string*                                                                          | :heavy_check_mark:                                                                  | List of trusted hardware manufacturers whose devices can create trusted provenance. |
| `Cas`                                                                               | []*string*                                                                          | :heavy_check_mark:                                                                  | List of trusted Certificate Authority certificates in PEM format.                   |
| `Policy`                                                                            | [components.Policy](../../models/components/policy.md)                              | :heavy_check_mark:                                                                  | Trust policy configuration defining default trust behavior.                         |