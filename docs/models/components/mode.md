# Mode

The signing mode to use.
* `server_measure`: The server streams the asset, calculates its hash, and embeds the manifest. Requires `manifest_json`. This is the primary signing mode.
* `client_hash`: The client provides the asset hash directly for offline signing. (Not yet implemented).



## Values

| Name                | Value               |
| ------------------- | ------------------- |
| `ModeServerMeasure` | server_measure      |
| `ModeClientHash`    | client_hash         |