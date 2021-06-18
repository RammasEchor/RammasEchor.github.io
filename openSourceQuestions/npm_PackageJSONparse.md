# How does *npm* parse the *package.json* file?

## 08 Jun 2021 - 14 Jun 2021

---

### Main page

[https://rammasechor.github.io/](https://rammasechor.github.io/)

---

## What is *npm*?

*npm* is an online repository of open source *Node.js* projects; it is also a command-line utility for interacting with this repository. Each project is called a *package*.

## Why *npm* parses the *package.json* file?

*npm* is also used for dependency management. A *Node.js* project may have a *package.json* file, which in turn contains all versions of the project's packages. *npm* can read this file and replicate the package environment specified with *npm install*.

## How does *npm* parse the *install* command?

I cloned the *npm* repo and started my search. I spy with my little eye a `Makefile`; but unfortunately, it is only used for creating the docs. But wait, when I run the file, at the top of the console output I get something interesting:

```bash
node bin/npm-cli.js install --no-audit --ignore-scripts
```

In `bin/npm-cli.js` we found only one line:

```javascript
require('../lib/cli.js')(process)
```

So, one file just redirects to another, but why? Running the first one give us an output, just like an installation of npm, but if the try to run the second we get nothing:

```bash
node bin/npm-cli.js --version
> 7.16.0
node lib/cli.js --version 
>
```

After an inconspicuous `console.log(process)` in `lib/cli.js`, we can see that `process` is the information about the current *Node.js* process:

```bash
process {
  version: 'v14.16.1',
  versions: {
    node: '14.16.1',
...
title: 'node',
  argv: [
    '/usr/local/bin/node',
    '/Users/.../npm/bin/npm-cli.js',
    '--version'
  ],
  pid: 70086,
  ppid: 67415,
  execPath: '/usr/local/bin/node',
...
```

So, we identified the entry point to *npm*, now, how do we proceed when `argv[2]` is `install`? In `lib/cli.js` there are various key lines:

```javascript
// We create the npm "object"
const npm = require('../lib/npm.js')
...
// Callback to function to execute the command.
npm.load(async er => {
    ...
    // cmd is the command to npm, if 'npm install', then cmd = install
    const cmd = npm.argv.shift()
    // find a function with that name
    const impl = npm.commands[cmd]
    if (impl)
        // If the command exists, execute it
        impl(npm.argv, errorHandler)
    else {
        // Some error handling if the function does not exists
        // Here the message "Unknown command, did you mean..." 
        // is created and displayed
    }
}
```

The indexation of `npm.commands[cmd]` is interesting. Lets see what this is about in `lib/npm.js`:

```javascript
// Object to intercept get() to whathever is assigned to.
const proxyCmds = new Proxy({}, {
  get: (target, cmd) => {
        // deref() completes the command if it is completable;
        // npm ins => npm install
        const actual = deref(cmd)
        // If the command exists...
        if (actual && !Reflect.has(target, actual)) {
            // We require the file named after the command.
            const Impl = require(`./${actual}.js`)
                // And we create a new command with our own npm object.
            const impl = new Impl(npm)
            ...
            // We create another proxy, to intercept the get call to
            // this object.
            target[actual] = new Proxy(
                // Object to get intercepted
                (args, cb) => npm[_runCmd](actual, impl, args, cb),
                // Handler to intercerpted get()
                {
                    get: (target, attr, receiver) => {
                                return Reflect.get(impl, attr, receiver)
                            },
                })
        }
        
        return target[actual]
    }
}

const npm = module.exports = new class extends EventEmitter { 
    constructor() {
        ...
        this.commands = proxyCmds
        ...
    }
}
```

So, `npm` has a `Proxy` object in it's `command` property; this `Proxy` intercepts the `get` method targeted to `commands` and returns a new `Proxy` that will intercept the `get()` call to that returned object. When we call this object, we enter `lib/install.js`, because `actual` resolves to `install`, and `Impl` ends up as follows:

```javascript
const Impl = require(`./install.js`)
```

## How does *npm*  find the *package.json* file?

Inside `lib/install.js` we have some key lines:

```javascript
class Install extends ArboristWorkspaceCmd {
    ...
    async install (args) {
        // Global installation for npm
        const globalTop = resolve(this.npm.globalDir, '..')
        const ignoreScripts = this.npm.config.get('ignore-scripts')
        // Does it have the arg global?
        const isGlobalInstall = this.npm.config.get('global')
        // If not, install locally. where holds the root of the directory
        // from where it is called.
        const where = isGlobalInstall ? globalTop : this.npm.prefix
        ...
    }
    ...
}
```

Were does `install` gets its *npm* object? Well, `ArboristWorkspaceCmd` extends `BaseCommand`, and `BaseCommand` has a *npm* object as part of its constructor args:

```javascript
class BaseCommand {
    constructor (npm) {
        this.wrapWidth = 80
        this.npm = npm
        this.workspaces = null
        this.workspacePaths = null
    }
    ...
}
```

*npm* finds the location of the global installation and the local directory. When installing, it runs a series of scripts:

```js
const runScript = require('@npmcli/run-script')
...
const scripts = [
        'preinstall',
        'install',
        'postinstall',
        'prepublish',
        'preprepare',
        'prepare',
        'postprepare',
      ]
      for (const event of scripts) {
        await runScript({
          path: where,
          args: [],
          scriptShell,
          stdio: 'inherit',
          stdioString: true,
          banner: log.level !== 'silent',
          event,
        })
      }
```

But they are not scripts, they are *package scripts*. A quick look at `https://github.com/npm/run-script` shows that they append `/package.json` to the path:

```js
const rpj = require('read-package-json-fast')
const runScriptPkg = require('./run-script-pkg.js')
...
return pkg ? runScriptPkg(options)
    // Aha! here we parse the data from the package.json file.
    // We just add the name to the local directory path.
    : rpj(path + '/package.json').then(pkg => runScriptPkg({...options, pkg}))
```

## So, how does *npm* read the *package.json* file?

Well, this is underwhelming, although it makes sense that they just use another library to parse the json. Lets see how far the rabbit hole goes. In `read-package-json-fast` we have:

```js
// Used to convert an action to a promise
const {promisify} = require('util')
// Filesystem interface
const fs = require('fs')
// We create a promise to read the file; how long may it be?
const readFile = promisify(fs.readFile)
// We use this package to parse the json file 
const parse = require('json-parse-even-better-errors')
// How do we parse it:
const rpj = path => readFile(path, 'utf8')
  .then(data => normalize(stripUnderscores(parse(data))))
  .catch(er => {
    er.path = path
    throw er
  })
```

So, `parse` is a simple function that removes some characters to avoid errors, and then tries to parse with the normal `JSON.parse()` function. Then, after parsing it, we call `runScriptPkg` with this json object as an argument. Each package is installed according to each package's script:

```js
const {scripts = {}, gypfile} = pkg
let cmd = null
if (options.cmd)
    cmd = options.cmd
else if (pkg.scripts && pkg.scripts[event])
    cmd = pkg.scripts[event] + args.map(a => ` ${JSON.stringify(a)}`).join('')
else if ( // If there is no preinstall or install script, default to rebuilding node-gyp packages.
    event === 'install' &&
    !scripts.install &&
    !scripts.preinstall &&
    gypfile !== false &&
    await isNodeGypPackage(path)
    )
        cmd = defaultGypInstallScript
```

So, to recap a few things:

- First, *npm* obtains the current directory, and the global installation path.
- Checks if the `install` command has more arguments, and if the packages being installed are meant to be global or local.
- *npm* parses the package's json info, and converts it to a json object.
- For each of those packages, they call their installing script in case there is one.
- After this, the package is installed, and ready to be imported and used.
