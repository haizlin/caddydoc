# http.basicauth
basicauth implements HTTP Basic Authentication. Basic Authentication can be used to protect directories and files with a username and password. Note that basic auth is not secure over plain HTTP. Use discretion when deciding what to protect with HTTP Basic Authentication.

When a user requests a resource that is protected, the browser will prompt the user for a username and password if they have not already supplied one. If the proper credentials are present in the Authorization header, the server will grant access to the resource and set the {user} placeholder to the value of the username. If the header is missing or the credentials are incorrect, the server will respond with HTTP 401 Unauthorized.

This directive allows use of .htpasswd files by prefixing the password argument with  htpasswd= and the path to the .htpasswd file to use. Support for .htpasswd is for legacy sites only and may be removed in the future; do not use .htpasswd with new sites.

## Syntax
```
basicauth path username password
```

path is the file or directory to protect
username is the username
password is the password
This syntax is convenient for protecting a single file or base path/directory with the default realm "Restricted". To protect multiple resources or to specify a realm, use the following variation:

```
basicauth username password {
    realm name
    resources
}
```

username is the username.
password is the password.
realm identifies the protection partition; it is optional and cannot be repeated. Realms are used to specify the space in which the protection applies. This can be convenient for user agents that are configured to remember authentication details (which is most browsers).
resources is a list of files/directories to protect, one per line.

## Examples
Protect all files in /secret so only Bob can access them with the password "hiccup":

```
basicauth /secret Bob hiccup
```

Protect multiple files and directories in the realm "Mary Lou's documents" so Mary Lou has access with her password "milkshakes":

```
basicauth "Mary Lou" milkshakes {
				realm "Mary Lou's documents"
    /notes-for-mary-lou.txt
    /marylou-files
    /another-file.txt
}
```