# npx shell Keyword Issue

## Problem Description

When using `npx` to execute binaries whose names match shell keywords, the execution fails with syntax errors. For example:

1. Running `npx @my-pkg/f` works as expected:
   ```
   $ npx @my-pkg/f
   hello from f
   ```

2. Running `npx @my-pkg/select` fails with:
   ```
   sh: -c: line 0: syntax error near unexpected token `newline'
   sh: -c: line 0: `select'
   ```

3. Similarly, running `npx @my-pkg/if` fails with:
   ```
   sh: -c: line 1: syntax error: unexpected end of file
   ```

Using single or double quotes around the binary name does not resolve the issue. For example:
```
$ npx '@my-pkg/select'
sh: -c: line 0: syntax error near unexpected token `newline'
sh: -c: line 0: `select'

$ npx "@my-pkg/if"
sh: -c: line 1: syntax error: unexpected end of file
```

## Potential Cause

This issue occurs because `npx` uses a shell to execute the binary, and if the binary name matches a shell keyword (e.g., `select`, `if`), the shell interprets it as part of a shell script rather than as a command. The binary name is not properly escaped or processed, leading to syntax errors.

Running the binary directly works without any issues. For example:
```
$ node_modules/.bin/select
hello from select
```

## Steps to Reproduce

1. Clone this REPO.
2. Without doing any `npm install`, run:
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
4. Alternatively, try running the binaries directly:
   ```
   node_modules/.bin/f
   node_modules/.bin/select
   node_modules/.bin/if
   ```
