# http.reauth  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.reauth** plugin when you download Caddy.
A common basis for authenticating with various and multiple authentication systems.

This came to be as we wanted to dynamically authenticate our docker registry against gitlab-ci and avoid storing credentials in gitlab while still permitting users to log in with their own credentials.

## Examples
Configuration
reauth {
    path /v2
    simple username=password,root=badpractice
    upstream url=https://accounts.example.com/check
    gitlab url=https://gitlab.example.com/
}
Authenticate access against /v2 with the following flow (in this order) 1 does the username and password match any of the given comma seperated credentials 2 basic http auth against https://accounts.example.com/check 3 against a gitlab project with the gitlab-ci-token user (docker login docker.example.com -u "$CIPROJECTPATH" -p "$CIBUILDTOKEN")