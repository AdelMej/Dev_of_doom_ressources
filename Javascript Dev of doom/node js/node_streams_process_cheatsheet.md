# Node.js Streams & `process` --- Cheat Sheet

## `process` basics (globals)

  Thing                  What it is
  ---------------------- -----------------------
  `process.stdin`        Readable stream
  `process.stdout`       Writable stream
  `process.stderr`       Writable stream
  `process.argv`         CLI arguments
  `process.exit(code)`   Exit program
  `process.env`          Environment variables

------------------------------------------------------------------------

## Stream types

  Type        Meaning         Example
  ----------- --------------- --------------------
  Readable    Produces data   stdin, file read
  Writable    Consumes data   stdout, file write
  Duplex      Read + write    socket
  Transform   Modifies data   gzip

------------------------------------------------------------------------

## Read from stdin (grader-proof)

``` js
let input = '';
process.stdin.setEncoding('utf8');

process.stdin.on('data', chunk => {
  input += chunk;
});

process.stdin.on('end', () => {
  console.log(input);
});
```

------------------------------------------------------------------------

## Line-by-line input

``` js
import readline from 'readline';

const rl = readline.createInterface({
  input: process.stdin,
  terminal: false
});

rl.on('line', line => {
  console.log(line);
});
```

------------------------------------------------------------------------

## Write to stdout

``` js
process.stdout.write("hello\n");
console.log("hello");
```

------------------------------------------------------------------------

## stderr

``` js
process.stderr.write("error\n");
console.error("error");
```

------------------------------------------------------------------------

## Stream events (Readable)

  Event     Meaning
  --------- ----------------
  `data`    chunk received
  `end`     EOF
  `error`   stream error

------------------------------------------------------------------------

## Writable methods

  Method      Purpose
  ----------- ---------------
  `write()`   write data
  `end()`     finish stream

------------------------------------------------------------------------

## Pipe

``` js
process.stdin.pipe(process.stdout);
```

Linux equivalent:

``` bash
cat file | cat
```

------------------------------------------------------------------------

## Important rules

-   Streams give **chunks**, not lines
-   Do not assume full JSON or strings
-   Accumulate if needed

------------------------------------------------------------------------

## Backpressure (advanced)

``` js
if (!process.stdout.write(data)) {
  process.stdout.once('drain', () => {
    // resume
  });
}
```

------------------------------------------------------------------------

## process.argv

``` js
console.log(process.argv);
```

------------------------------------------------------------------------

## Exit codes

``` js
process.exit(0); // success
process.exit(1); // error
```

------------------------------------------------------------------------

## Common mistakes

-   Assuming one chunk = one line
-   Forgetting newline in output
-   Mixing ESM and CommonJS
-   Using browser APIs

------------------------------------------------------------------------

## Unix mental model

  Node     Unix
  -------- -----------------
  stream   file descriptor
  chunk    buffer
  pipe     `|`
  end      EOF

------------------------------------------------------------------------

## Auto-grader survival tips

-   Use `console.log` unless specified
-   Match output exactly
-   No extra prints
