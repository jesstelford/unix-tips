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
