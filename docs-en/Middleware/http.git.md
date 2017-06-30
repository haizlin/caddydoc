# http.git  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.git** plugin when you download Caddy.
The git plugin makes it possible to deploy your site with a simple git push.

The git directive starts a service routine that runs during the lifetime of the server. When the service starts, it clones the repository. While the server is still up, it pulls the latest every so often. You can also set up a webhook to pull immediately after a push. In regular git fashion, a pull only includes changes, so it is very efficient.

## Examples
**Basic syntax**
```
git repo [path]
```
repo is the URL to the repository; SSH and HTTPS URLs are supported.
path is the path, relative to site root, to clone the repository into; default is site root.
This simplified syntax pulls from master every 3600 seconds (1 hour) and only works for public repositories.

**Full syntax**
```
git [repo path] {
    repo        repo
    path        path
    branch      branch
    key         key
    interval    interval
    clone_args  args
    pull_args   args
    hook        path secret
    hook_type   type
    then        command [args...]
    then_long   command [args...]
}
```
* `repo` is the URL to the repository; SSH and HTTPS URLs are supported.
* `path` is the path to clone the repository into; default is site root. It can be absolute or relative (to site root).
* `branch` is the branch or tag to pull; default is master branch. {latest} is a placeholder for latest tag which ensures the most recent tag is always pulled.
* `key` is the path to the SSH private key; only required for private repositories.
* `interval is the number of seconds between pulls; default is 3600 (1 hour), minimum 5. An interval of -1 disables periodic pull.
* `clone_args` is the additional cli args to pass to git clone e.g. --depth=1. git clone is called when the source is being fetched the first time.
* `pull_args` is the additional cli args to pass to git pull e.g. -s recursive -X theirs.  git pull is used when the source is being updated.
* `path` and `secret` are used to create a webhook which pulls the latest right after a push. This is limited to the supported webhooks. secret is currently supported for GitHub and Travis hooks only.
* `type` is webhook type to use. The webhook type is auto detected by default but it can be explicitly set to one of the supported webhooks. This is a requirement for generic webhook.
* `command` is a command to execute after successful pull; followed by args which are any arguments to pass to the command. You can have multiple lines of this for multiple commands. then_long is for long executing commands that should run in background.

Each property in the block is optional. The path and repo may be specified on the first line, as in the first syntax, or they may be specified in the block with other values.

**Basic example**
```
git github.com/user/myproject subfolder
```
Public repository pulled into the subfolder directory in the site root.

**More control**
```
git {
    repo     git@github.com:user/myproject
    branch   v1.0
    key      /home/user/.ssh/id_rsa
    path     subfolder
    interval 86400
}
```
Private repository pulled into the subfolder directory with tag v1.0 once per day.

**Running a command after pull**
```
git github.com/user/site {
    path  ../
    then  hugo --destination=/home/user/hugosite/public
}
```
This example generates a static site with Hugo after each pull.

**Specifying a webhook**
```
git git@github.com:user/site {
    hook /webhook secret-password
}
```
/webhook is the path and secret-password is the hook secret (if applicable). Webhooks are supported for GitHub, Gitlab, BitBucket, Travis and Gogs.

You might need quotes around your secret if it contains special characters.

**Generic webhook**
```
{
    "ref" : "refs/heads/branch"
}
```
This is the expected payload for a generic webhook. branch is branch name e.g. master.