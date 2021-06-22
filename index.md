# Week 11 - Open source / Read others people's code (2)

## 15 Jun 2021 - 21 Jun 2021

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

In the open source codebase they have an `error-message` used for constructing messages for output. For the proposal, I did two things:

- Before the error in `search` is thrown about no arguments passed, I output a JSON to `STDOUT` with information about the error, only for that error.
- Before the error is thrown in `ls` about a dependency that cannot be found, I add to the JSON info an error with all the properties that are also sent to `STDERR`.

This was a little tricky because `npm` only allows output in a method called `npm.output()`, and in no other place.

## The Go Language - Simplicity is complicated

When I was researching about the Go language, I found about this little video that goes about why Go is the way it is.

Some languages integrate features from another languages as time passes. This leads to all languages down a path where the end is that the only thing that makes them different is the name.

Go is intended to be a simple language. If a language has too many features, you waste time choosing which ones to use. Preferable to have just one way, or at least fewer, simpler ways. Readability is paramount.

If you need to recreate a thought process to read code, it does not have readability as it's principal purpose. The code is harder to understand just because it is using a more complicated language. Simplicity is the art of hiding complexity.

## A tour of Go

I will try to put my takeaways here from this little tutorial, for later reference.

- Every go program is made of *packages*
- Program starts running in package main
- `import` imports packages
- Only names that start with uppercase letters are imported
- Functions:
  - `func add(x int, y int) int`
  - `func add(x, y int) int`
  - `func add(x int, y int) (int, string)`
  - Named return values: `func add(x int, y int) (x, y int)`
    - All named return values are returned if `return` without args

- Variables:
  - `var x, y int`
  - `var x, y int = 1, 2`
  - Inferred values: `x := 1` creates an int
  - Casts: `y := uint(24.5)`

- Flow control:
  - Only one construct for loops, `for`:
    - `for i := 0; i < 10; i++ { sum += i }`
    - Like while: `for i < 10 { i += i }`
  - `if` construct:
    - Can have initialization: `if v := math.Pow(x, n); v < lim { return v }`
    - `if v := math.Pow(x, n); v < lim {  return v } else {  fmt.Printf("%g >= %g\n", v, lim) }`
  - `switch`:
    - Runs the first case that matches and nothing else.
    - Has initialization
    - `switch i := 2, i { case 2: do.somth() default: do.smthelse() }`
    - Can be used for long `if else` chains:
      - `switch { case i < 1 : do.s() case i > 1 : do.h() }`
  - `defer`:
    - Defers the execution of the function until the scope exits.
    - Defer stacks the functions, *last-in-first-out*.

- Pointers:
  - Holds the memory address of a value.
  - Pointer to int: `var p *int`
  - THe `*` operator access the underlying value.

- Structs:
  - Collection of fields
  - Access using a dot: `v.X`

```go
type Vertex struct {
 X int
 Y int
}
```

- Pointers to structs
  - `p := &v`
  - `p.X`

- Struct literals
  - `Vertex{1, 2}`
  - `Vertex{X:1}` where `Y:0` is implicit.

- Arrays:
  - Fixed size.
  - `var a [10]int`

- Slices:
  - Dynamic.
  - `var b []int`

- Arrays and slices:
  - A slice is a reference to a subset of elements of an array.
  - `a[1:4]`, `a[:4]`, `a[:]` creates a slice.
  - Changing the elements of a slice modifies the corresponding array.
  - Other slices that reference that array will see those changes.
  - This is an array: `[3]bool{true, true, false}`
  - This creates an array and then builds a slice that references it: `[]bool{true, true, false}`
  
- `len(s)` and `cap(s)`
  - Length of a slice is the number of elements it contains
  - Number of elements in the underlying array, counting from the first element in the slice

- Dynamically sized arrays (slices with make):
  - `a := make([]int, 5)`
  
- Slices can have more slices in them
- Append to a slice: `append(s, 0)`; `s` is the slice, `0` the elements.
- `for range` iterates over a slice or map: `for i, v:= range slice {}`
  - index and copy of value
  - Skip the index: `_, v := range s`

## Conclusions

This week I learned a lot about big codebases; I learned that it helps immensely to be able to run the project instead of only reading the code. Unfortunately I choose as a first project to work on something that did not have good communication towards newcomers (I didn't learn this until later). This is not in a demeaning manner, I'm sure the maintainers have a lot on their plate right now, and everybody is busy, and can't respond to everything. But I'm happy that I followed the education team advice, and just went for the issue. Most probably my pull request won't be accepted, but hey, I tried and that is what matters.

## All blogs

| Blog | Info |
| --- | --- |
| [Week 1 - Innovation and hard\smart work](/Week_Pages/Week1_April.md) | 05 Apr 2021 - 12 Apr 2021 |
| [Week 2 - Polyglot Programming](/Week_Pages/Week2_April.md) | 13 Apr 2021 - 19 Apr 2021 |
| [Week 3 - Fancy Topics](/Week_Pages/Week3_April.md) | 20 Apr 2021 - 26 Apr 2021 |
| [Week 4 - It's all about science](/Week_Pages/Week4_April.md) | 27 Apr 2021 - 03 May 2021 |
| [Pretotypes](/Pretotypes/Pretotypes_April2021.md) | My pretotypes for week 4 (*It's all about science*) |
| [Month 1 - Reset Phase](/Month_Pages/Month1_April.md) | 05 Apr 2021 - 03 May 2021 |
| [Week 6 - Building something from scratch (1)](/Week_Pages/Week6_May.md) | 11 May 2021 - 17 May 2021 |
| [Week 7 - Building something from scratch (2)](/Week_Pages/Week7_May.md) | 18 May 2021 - 24 May 2021 |
| [Week 8 - Building something from scratch (3)](/Week_Pages/Week8_May.md) | 25 May 2021 - 31 May 2021 |
| [Week 9 - Building something from scratch (4)](/Week_Pages/Week9_Jun.md) | 01 Jun 2021 - 07 Jun 2021 |
| [Month 2 - Building something from scratch](/Month_Pages/Month2_May.md) | 11 May 2021 - 07 Jun 2021 |
| [Week 10 - Open source / Read others people's code (1)](/Week_Pages/Week10_Jun.md) | 08 Jun 2021 - 14 Jun 2021 |
| [Week 11 - Open source / Read others people's code (2)](/Week_Pages/Week11_Jun.md) | 15 Jun 2021 - 21 Jun 2021 |
