# Unix Tips

## Terminal-fu

### Recursive search & replace with confirmation

Can be done with a combo of `vim` and `ag`: (http://unix.stackexchange.com/a/20255/94874)

```
ag -l --ignore-dir="node_modules" "var" ./ | xargs -l vim -u NONE -c '%s/var/let/gc' -c 'wq'
```

**Optional Extra**

The switch `-u NONE` on the `vim` command will ignore your `vimrc` config files.
This is useful if you get parsing errors, etc, due to syntax highlighters or slow linters.

**How it works**

This works by first using `ag` to find the list of files containing the pattern `"var"`,
then passes each file into `vim` one at a time (`xargs -l`),
asking for confirmation of the command `%s` (which is the replacement command),
followed by the command `wq` (which saves and quits after replacing).

### Recursive `rm` of folder by name

For example; after doing JS dev for a while,
there will be loads of `node_modules` folders hidden away taking up disk space.

See what will be deleted:

```
find ./ -type d -path "./**/node_modules"
```

Perform the deletion with confirmation:

```
find ./ -type d -path "./**/node_modules" -exec rm -r {} \;
```

**How it works**

`-type d` means _directories_.

`-path X` expands `X` as a glob, and matches only against that path.

`-exec` executes the `rm` command,
where `{}` is replaced with the discovered file path. (`\;` ends the `-exec` option).
