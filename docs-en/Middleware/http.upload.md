# http.upload  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.upload** plugin when you download Caddy.
Upload files using an API and HTTP method POST or PUT.

Handy for uploads of the likes of build artifacts, downloads-to-be, or the odd file using tools like curl.

## Examples
quick and simple upload
curl \
  -T /etc/os-release \
  https://127.0.0.1/wp-upload/os-release
Uploads os-release into path wp-upload.

quick and dirty delete
curl -X DELETE \
  https://127.0.0.1/wp-upload/os-release
Deletes wp-upload/os-release if it exists.

move or rename a file
curl -X MOVE \
  -H "destination: /wp-upload/old-release" \
  https://127.0.0.1/wp-upload/os-release
The file will be henceforth available under the new name.