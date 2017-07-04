Placeholders
Some directives allow you to use placeholders in your Caddyfile to fill out a value differently for every request. For example, the value {path} would be replaced by the path portion of the request URL. These are also called replaceable values.

These placeholders only work on directives that support them. Check the documentation for your directive to see if placeholders are supported.
Request Placeholders
These values are obtained from the request.

Placeholder	Value
{~cookie}	The value of a cookie, where "cookie" is the cookie name
{dir}	The directory of the requested file (from request URI)
{file}	The name of the requested file (from request URI)
{fragment}	The last part of the URL starting with "#"
{>Header}	Any request header, where "Header" is the header field name
{host}	The host value on the request
{hostname}	The name of the host machine that is processing the request
{hostonly}	Same as {host} but without port information
{method}	The request method (GET, POST, etc.)
{mitm}	Whether HTTPS interception is likely, unlikely, or unknown
{path}	The path portion of the original request URI (does not include query string or fragment)
{path_escaped}	Query-escaped variant of {path}
{port}	The client's port
{proto}	The protocol string (e.g. "HTTP/1.1")
{query}	The query string portion of the URL, without leading "?"
{query_escaped}	The query-escaped variant of {query}
{?key}	The value of the "key" argument from the query string
{remote}	The client's IP address
{request}	The entire HTTP request (sans body), compacted to one line
{request_body}	The request body, compacted to one line (max length 100 KB; JSON or XML only)
{rewrite_path}	Same as {path}, but is the value of the path after rewrites
{rewrite_path_escaped}	Query-escaped variant of {rewrite_path}
{rewrite_uri}	The request URI after any rewrite has occured (includes path, query string, and fragment)
{rewrite_uri_escaped}	The query-escaped variant of {rewrite_uri}
{scheme}	The protocol/scheme used (usually http or https)
{uri}	The request URI (includes path, query string, and fragment)
{uri_escaped}	The query-escaped variant of {uri}
{when}	Timestamp in the format 02/Jan/2006:15:04:05 -0700 in local time
{when_iso}	Timestamp in the format 2006-01-02T15:04:05Z in UTC
Response Placeholders
These values are obtained from the response, and are only implemented with some directives. Make sure your directive supports response placeholders before attempting to use them.

Placeholder	Value
{latency}	Approximate time in a human-readable format the server spent handling the request
{latency_ms}	Approximate time in milliseconds the server spent handling the request
{size}	The size of the response body
{status}	The HTTP status code of the response
