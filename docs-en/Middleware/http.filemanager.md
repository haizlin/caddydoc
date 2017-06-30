# http.filemanager  `PLUGIN`
> This feature does not come with Caddy by default. To get it, select the **http.filemanager** plugin when you download Caddy.
filemanager is an extension based on browse middleware. It provides a file managing interface within the specified directory and it can be used to upload, delete, preview and rename your files within that directory.

## Examples
**Basic Usage**
```
filemanager
```
Show the directory where Caddy is being executed at the root of the domain.

**WebDav Only**
```
filemanager {
    webdav /
}
```
Replace the GUI and use only WebDav.

**Browse specific path**
```
filemanager {
    show foo/
}
```
Show the content of foo at the root of the domain.

**Passwords and Permissions**
```
basicauth /admin admin pass
basicauth /admin manager pass
basicauth /admin editor pass

filemanager /admin {
    show           ./
    allow_commands false
    admin:
    allow_commands true
    allow_command  rm
    allow_command  mv
    allow          dotfiles
    manager:
    block          /admin/financial
    editor:
    allow_new      false
    block          /admin/financial
}
```
You have three users: an administrator, a manager and an editor. The administrator can do everything and has access to the commands rm and mv because he is a geeky. The manager, doesn't have access to commands, but can create and edit files. The editor can only edit files. He can't even create new ones, because he will only edit the files after the manager creates them for him. Both the editor and the manager won't have access to the financial folder.

