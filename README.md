# Unix Tips

## Terminal-fu

### Recursive search & replace with confirmation

Can be done with a combo of `vim` and `ag`: (http://unix.stackexchange.com/a/20255/94874)

```
SEARCH="var"; REPLACE="let"; ag -l --ignore-dir="node_modules" "$SEARCH" ./ | xargs -o -L 1 vim -u NONE -c "%s/$SEARCH/$REPLACE/gc" -c 'wq'
```

**How it works**

This works by first using `ag` to find the list of files containing the pattern `"var"`,
then passes each file into `vim` one at a time (`xargs -L 1`),
asking for confirmation of the command `%s` (which is the replacement command),
followed by the command `wq` (which saves and quits after replacing).

The switch `-u NONE` on the `vim` command will ignore your `vimrc` config files.
This is useful if you get parsing errors, etc, due to syntax highlighters or slow linters.

### Recursive `rm` of folder by name

For example; after doing JS dev for a while,
there will be loads of `node_modules` folders hidden away taking up disk space.

See what will be deleted:

```
find ./ -type d -path "./**/node_modules"
```

Perform the deletion with confirmation:

```
find ./ -type d -path "./**/node_modules" -exec sh -c 'read -p "delete {}? [y to proceed] " -n 1 -r REPLY && echo && [[ $REPLY =~ ^[Yy]$ ]] && rm -rf {}' \;
```

**How it works**

`-type d` means _directories_.

`-path X` expands `X` as a glob, and matches only against that path.

`-exec sh -c` runs a new shell instance to execute the following commands
where `{}` is replaced with the discovered file path. (`\;` ends the `-exec` option).

The shell command [asks for user permission](http://stackoverflow.com/a/1885534/473961)
before proceeding to do an `rm -rf` on that directory.
