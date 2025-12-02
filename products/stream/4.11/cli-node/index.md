# node


When you run `node` with no options, it displays a command prompt, as shown here:

```text
>
```

To execute a JavaScript file, you can enter path/file name at the prompt.

With the `-v` option, prints the version of NodeJS that is running.

With `-e`, evaluates a string. Write to console to see the output. For example:

**Usage:**

```text
./cribl node -e 'console.log(Date.now())'
./cribl node -v
```

**Sample response:**

```text
./cribl node -e 'console.log(Date.now())'
1629740667695
```

```text
v14.15.1
```

**Arguments:**

| Option | Definition |
| ------ | ---------- |
| `-e <eval>` | String to evaluate. |
| `-v` | Prints NodeJS version. |