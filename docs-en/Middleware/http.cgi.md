# http.cgi  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.cgi** plugin when you download Caddy.
This plugin implements the Common Gateway Interface (CGI) for Caddy. It lets you generate dynamic content on your website by means of command line scripts. To collect information about the inbound HTTP request, your script examines certain environment variables such as PATH_INFO and QUERY_STRING. Then, to return a dynamically generated web page to the client, your script simply writes content to standard output. In the case of POST requests, your script reads additional inbound content from standard input.

## Examples
**Simple CGI script**
```
In the Caddyfile:

  cgi /report /usr/local/cgi-bin/report

In /usr/local/cgi-bin/report:

  #!/bin/bash

  printf "Content-type: text/plain\n\n"

  printf "PATH_INFO    [%s]\n" $PATH_INFO
  printf "QUERY_STRING [%s]\n" $QUERY_STRING

  exit 0
```

When a request such as https://example.com/report or https://example.com/report/weekly arrives, the cgi middleware will detect the match and invoke the script named /usr/local/cgi-bin/report.

The environment variables PATH_INFO and QUERY_STRING are populated and passed to the script automatically. There are a number of other standard CGI variables included that are described in the documentation. If you need to pass any special environment variables or allow any environment variables that are part of Caddy's process to pass to your script, you will need to use the advanced directive syntax described in the documentation.