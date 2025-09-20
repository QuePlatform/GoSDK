# GetPresignedURLResponse


## Fields

| Field                                                                     | Type                                                                      | Required                                                                  | Description                                                               |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| `HTTPMeta`                                                                | [components.HTTPMetadata](../../models/components/httpmetadata.md)        | :heavy_check_mark:                                                        | N/A                                                                       |
| `PresignResponse`                                                         | [*components.PresignResponse](../../models/components/presignresponse.md) | :heavy_minus_sign:                                                        | A presigned URL and object key were successfully generated.               |
| `Headers`                                                                 | map[string][]*string*                                                     | :heavy_check_mark:                                                        | N/A                                                                       |