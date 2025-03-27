# Issue: `npx` Fails to Execute Binaries Named After Shell Keywords

When using `npx` to execute binaries whose names match shell keywords (e.g., `select`, `if`), the execution fails with syntax errors. This issue occurs even when the binary name is quoted using single or double quotes.

The reproduction repo can be found: [npx-shell-keyword-issue](https://github.com/ycmjason/npx-shell-keyword-issue).

## Description

`npx` fails to execute binaries named after shell keywords, producing syntax errors. For example:
```
$ npx @my-pkg/select
sh: -c: line 0: syntax error near unexpected token `newline'
sh: -c: line 0: `select'

$ npx "@my-pkg/if"
sh: -c: line 1: syntax error: unexpected end of file
```

Running the binaries directly using their full paths works without any issues:
```
$ node_modules/.bin/select
hello from select
```

## Steps to Reproduce

1. Clone the following repository: [npx-shell-keyword-issue](https://github.com/ycmjason/npx-shell-keyword-issue).
2. Without running `npm install`, execute the following commands:
   ```
   npx @my-pkg/f
   npx @my-pkg/select
   npx @my-pkg/if
   ```
3. Try quoting the binary names:
   ```
   npx '@my-pkg/select'
   npx "@my-pkg/if"
   ```
4. Alternatively, run the binaries directly:
   ```
   node_modules/.bin/f
   node_modules/.bin/select
   node_modules/.bin/if
   ```

## Expected Behavior

`npx` should execute binaries correctly, even if their names match shell keywords.

## Actual Behavior

`npx` fails to execute binaries named after shell keywords, producing syntax errors.

## Repository

A minimal reproduction of this issue is available at: [npx-shell-keyword-issue](https://github.com/ycmjason/npx-shell-keyword-issue).
