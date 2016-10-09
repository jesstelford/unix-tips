# Unix Tips

## Terminal-fu

### Recursive search & replace with confirmation

Can be done with a combo of `vim` and `ag`: (http://unix.stackexchange.com/a/20255/94874)

```
ag -l --ignore-dir="node_modules" "var" ./ | xargs -l vim -c '%s/var/let/gc' -c 'wq'
```

This works by first using `ag` to find the list of files containing the pattern `"var"`,
then passes each file into `vim` one at a time (`xargs -l`),
asking for confirmation of the command `%s` (which is the replacement command),
followed by the command `wq` (which saves and quits after replacing).
