# Week 11 - Open source / Read others people's code (2)

## 15 Jun 2021 - 21 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## *npm* contribution

### [BUG] NPM 7.x broke the "--json" CLI parameter #2740

[Link to issue](https://github.com/npm/cli/issues/2740)

#### Context

*npm* has an argument `--json` for certain options, which outputs information about the option used. For example:

```console
$ npm search --json
npm ERR! search must be called with arguments
{
  "error": {
    "code": null,
    "summary": "search must be called with arguments",
    "detail": ""
  }
}

npm ERR! A complete log of this run can be found in:
```

In *npm 6.x*, the JSON object is sent to `STDOUT`. If we send the `STDOUT` and `STDERR` streams to the files `out` and `error` respectively,

```console
$ npm --version
6.14.12
$ npm search --json >out 2>error
$
```

in the `out` file we get:

```bash
{
  "error": {
    "summary": "search must be called with arguments",
    "detail": ""
  }
}
```

and if we check the `error` file we get:

```bash
npm ERR! search must be called with arguments

npm ERR! A complete log of this run can be found in:
...
```

But if I do the same for *npm 7.x* (in the cloned repo),

```console
$ node bin/npm-cli.js --version
7.17.0
$ npm search --json >out 2>error
$
```

 we get nothing in `out`, and this in `error`:

 ```bash
npm ERR! search must be called with arguments
{
  "error": {
    "code": null,
    "summary": "search must be called with arguments",
    "detail": ""
  }
}

npm ERR! A complete log of this run can be found in:
...
```

So, now the problem is *not* to just set the output to `STDOUT`, but to check *why* is broken in the first place. It turns out this problem is a direct consequence to a fix for *another* bug. This leaves us in a tricky place. Do we fix the most recent bug, or do we leave it? Well, we can't just leave it as it is.

The fix was in response to an issue concerning two JSON objects in the output ([Link to that issue](https://github.com/npm/cli/issues/2150)):

```js
{
  "name": "temp",
  "problems": [
    "missing: foo@1.0.0, required by temp@"
  ],
  "dependencies": {
    "foo": {
      "required": "1.0.0",
      "missing": true,
      "problems": [
        "missing: foo@1.0.0, required by temp@"
      ]
    }
  }
}
{
  "error": {
    "code": "ELSPROBLEMS",
    "summary": "missing: foo@1.0.0, required by temp@",
    "detail": ""
  }
}
```

Basically, it makes streamlined parsing of the output to a JSON object impossible because there are two top-level JSON objects (*streamlined* is the keyword here). The fix in response to this was to log that last `error` JSON to `STDERR`, instead of `STDOUT`.

#### Proposal

Output only one JSON object to `STDOUT`, with the `error` object embedded within. The error JSON object is also sent to `STDERR`.

[Link to pull request](https://github.com/npm/cli/pull/3437)

## The Go Language - Simplicity is complicated

When I was researching about the Go language, I found about this little video that goes about why Go is the way it is.

Some languages integrate features from another languages as time passes. This leads to all languages down a path where the end is that the only thing that makes them different is the name.

Go is intended to be a simple language. If a language has too many features, you waste time choosing which ones to use. Preferable to have just one way, or at least fewer, simpler ways. Readability is paramount.

If you need to recreate a thought process to read code, it does not have readability as it's principal purpose. The code is harder to understand just because it is using a more complicated language. Simplicity is the art of hiding complexity.
