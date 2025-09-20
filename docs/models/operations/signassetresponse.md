# SignAssetResponse


## Fields

| Field                                                               | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `HTTPMeta`                                                          | [components.HTTPMetadata](../../models/components/httpmetadata.md)  | :heavy_check_mark:                                                  | N/A                                                                 |
| `SignResponse`                                                      | [*components.SignResponse](../../models/components/signresponse.md) | :heavy_minus_sign:                                                  | The asset was successfully signed.                                  |
| `Headers`                                                           | map[string][]*string*                                               | :heavy_check_mark:                                                  | N/A                                                                 |