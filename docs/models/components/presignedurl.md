# PresignedURL

An asset accessible via HTTP/HTTPS URL. The URL must be enabled via the ALLOW_URL_ASSETS environment variable. The service will stream the file to temporary storage during processing.


## Fields

| Field                                                         | Type                                                          | Required                                                      | Description                                                   | Example                                                       |
| ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- | ------------------------------------------------------------- |
| `URL`                                                         | *string*                                                      | :heavy_check_mark:                                            | The HTTP/HTTPS URL of the asset. Must be publicly accessible. | https://example.com/assets/photo.jpg                          |