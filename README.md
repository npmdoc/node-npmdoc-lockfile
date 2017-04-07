# api documentation for  [lockfile (v1.0.3)](https://github.com/npm/lockfile#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-lockfile.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-lockfile) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-lockfile.svg)](https://travis-ci.org/npmdoc/node-npmdoc-lockfile)
#### A very polite lock file utility, which endeavors to not litter, and to wait patiently for others.

[![NPM](https://nodei.co/npm/lockfile.png?downloads=true)](https://www.npmjs.com/package/lockfile)

[![apidoc](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.buildNpmdoc.browser.%2Fhome%2Ftravis%2Fbuild%2Fnpmdoc%2Fnode-npmdoc-lockfile%2Ftmp%2Fbuild%2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-lockfile/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Isaac Z. Schlueter",
        "email": "i@izs.me",
        "url": "http://blog.izs.me/"
    },
    "bugs": {
        "url": "https://github.com/npm/lockfile/issues"
    },
    "dependencies": {},
    "description": "A very polite lock file utility, which endeavors to not litter, and to wait patiently for others.",
    "devDependencies": {
        "tap": "^7.1.2",
        "touch": "0"
    },
    "directories": {
        "test": "test"
    },
    "dist": {
        "shasum": "2638fc39a0331e9cac1a04b71799931c9c50df79",
        "tarball": "https://registry.npmjs.org/lockfile/-/lockfile-1.0.3.tgz"
    },
    "gitHead": "96549505fcca4b8b0ff0e833719720bd463306d7",
    "homepage": "https://github.com/npm/lockfile#readme",
    "keywords": [
        "lockfile",
        "lock",
        "file",
        "fs",
        "O_EXCL"
    ],
    "license": "ISC",
    "main": "lockfile.js",
    "maintainers": [
        {
            "name": "trevorburnham",
            "email": "trevorburnham@gmail.com"
        },
        {
            "name": "isaacs",
            "email": "i@izs.me"
        }
    ],
    "name": "lockfile",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/npm/lockfile.git"
    },
    "scripts": {
        "changelog": "bash gen-changelog.sh",
        "postversion": "npm run changelog && git add CHANGELOG.md && git commit -m 'update changelog - '${npm_package_version}",
        "test": "tap test/*.js --cov"
    },
    "version": "1.0.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module lockfile](#apidoc.module.lockfile)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>check (path, opts, cb)](#apidoc.element.lockfile.check)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>checkSync (path, opts)](#apidoc.element.lockfile.checkSync)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>lock (path, opts, cb)](#apidoc.element.lockfile.lock)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>lockSync (path, opts)](#apidoc.element.lockfile.lockSync)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>unlock (path, cb)](#apidoc.element.lockfile.unlock)
1.  [function <span class="apidocSignatureSpan">lockfile.</span>unlockSync (path)](#apidoc.element.lockfile.unlockSync)
1.  string <span class="apidocSignatureSpan">lockfile.</span>filetime



# <a name="apidoc.module.lockfile"></a>[module lockfile](#apidoc.module.lockfile)

#### <a name="apidoc.element.lockfile.check"></a>[function <span class="apidocSignatureSpan">lockfile.</span>check (path, opts, cb)](#apidoc.element.lockfile.check)
- description and source-code
```javascript
check = function (path, opts, cb) {
  if (typeof opts === 'function') cb = opts, opts = {}
  debug('check', path, opts)
  fs.open(path, 'r', function (er, fd) {
    if (er) {
      if (er.code !== 'ENOENT') return cb(er)
      return cb(null, false)
    }

    if (!opts.stale) {
      return fs.close(fd, function (er) {
        return cb(er, true)
      })
    }

    fs.fstat(fd, function (er, st) {
      if (er) return fs.close(fd, function (er2) {
        return cb(er)
      })

      fs.close(fd, function (er) {
        var age = Date.now() - st[exports.filetime].getTime()
        return cb(er, age <= opts.stale)
      })
    })
  })
}
```
- example usage
```shell
...

Close and unlink the lockfile.

### lockFile.unlockSync(path)

Close and unlink the lockfile.

### lockFile.check(path, [opts], cb)

Check if the lockfile is locked and not stale.

Callback is called with 'cb(error, isLocked)'.

### lockFile.checkSync(path, [opts])
...
```

#### <a name="apidoc.element.lockfile.checkSync"></a>[function <span class="apidocSignatureSpan">lockfile.</span>checkSync (path, opts)](#apidoc.element.lockfile.checkSync)
- description and source-code
```javascript
checkSync = function (path, opts) {
  opts = opts || {}
  debug('checkSync', path, opts)
  if (opts.wait) {
    throw new Error('opts.wait not supported sync for obvious reasons')
  }

  try {
    var fd = fs.openSync(path, 'r')
  } catch (er) {
    if (er.code !== 'ENOENT') throw er
    return false
  }

  if (!opts.stale) {
    try { fs.closeSync(fd) } catch (er) {}
    return true
  }

  // file exists.  however, might be stale
  if (opts.stale) {
    try {
      var st = fs.fstatSync(fd)
    } finally {
      fs.closeSync(fd)
    }
    var age = Date.now() - st[exports.filetime].getTime()
    return (age <= opts.stale)
  }
}
```
- example usage
```shell
...

### lockFile.check(path, [opts], cb)

Check if the lockfile is locked and not stale.

Callback is called with 'cb(error, isLocked)'.

### lockFile.checkSync(path, [opts])

Check if the lockfile is locked and not stale.

Returns boolean.

## Options
...
```

#### <a name="apidoc.element.lockfile.lock"></a>[function <span class="apidocSignatureSpan">lockfile.</span>lock (path, opts, cb)](#apidoc.element.lockfile.lock)
- description and source-code
```javascript
lock = function (path, opts, cb) {
  if (typeof opts === 'function') cb = opts, opts = {}
  opts.req = opts.req || req++
  debug('lock', path, opts)
  opts.start = opts.start || Date.now()

  if (typeof opts.retries === 'number' && opts.retries > 0) {
    debug('has retries', opts.retries)
    var retries = opts.retries
    opts.retries = 0
    cb = (function (orig) { return function cb (er, fd) {
      debug('retry-mutated callback')
      retries -= 1
      if (!er || retries < 0) return orig(er, fd)

      debug('lock retry', path, opts)

      if (opts.retryWait) setTimeout(retry, opts.retryWait)
      else retry()

      function retry () {
        opts.start = Date.now()
        debug('retrying', opts.start)
        exports.lock(path, opts, cb)
      }
    }})(cb)
  }

  // try to engage the lock.
  // if this succeeds, then we're in business.
  fs.open(path, wx, function (er, fd) {
    if (!er) {
      debug('locked', path, fd)
      locks[path] = fd
      return fs.close(fd, function () {
        return cb()
      })
    }

    // something other than "currently locked"
    // maybe eperm or something.
    if (er.code !== 'EEXIST') return cb(er)

    // someone's got this one.  see if it's valid.
    if (!opts.stale) return notStale(er, path, opts, cb)

    return maybeStale(er, path, opts, false, cb)
  })
}
```
- example usage
```shell
...

## Usage

'''javascript
var lockFile = require('lockfile')

// opts is optional, and defaults to {}
lockFile.lock('some-file.lock', opts, function (er) {
// if the er happens, then it failed to acquire a lock.
// if there was not an error, then the file was created,
// and won't be deleted until we unlock it.

// do my stuff, free of interruptions
// then, some time later, do:
lockFile.unlock('some-file.lock', function (er) {
...
```

#### <a name="apidoc.element.lockfile.lockSync"></a>[function <span class="apidocSignatureSpan">lockfile.</span>lockSync (path, opts)](#apidoc.element.lockfile.lockSync)
- description and source-code
```javascript
lockSync = function (path, opts) {
  opts = opts || {}
  opts.req = opts.req || req++
  debug('lockSync', path, opts)
  if (opts.wait || opts.retryWait) {
    throw new Error('opts.wait not supported sync for obvious reasons')
  }

  try {
    var fd = fs.openSync(path, wx)
    locks[path] = fd
    try { fs.closeSync(fd) } catch (er) {}
    debug('locked sync!', path, fd)
    return
  } catch (er) {
    if (er.code !== 'EEXIST') return retryThrow(path, opts, er)

    if (opts.stale) {
      var st = fs.statSync(path)
      var ct = st[exports.filetime].getTime()
      if (!(ct % 1000) && (opts.stale % 1000)) {
        // probably don't have subsecond resolution.
        // round up the staleness indicator.
        // Yes, this will be wrong 1/1000 times on platforms
        // with subsecond stat precision, but that's acceptable
        // in exchange for not mistakenly removing locks on
        // most other systems.
        opts.stale = 1000 * Math.ceil(opts.stale / 1000)
      }
      var age = Date.now() - ct
      if (age > opts.stale) {
        debug('lockSync stale', path, opts, age)
        exports.unlockSync(path)
        return exports.lockSync(path, opts)
      }
    }

    // failed to lock!
    debug('failed to lock', path, opts, er)
    return retryThrow(path, opts, er)
  }
}
```
- example usage
```shell
...
possible for certain types of failures to cause this to fail, but a best
effort is made to not be a litterbug.

### lockFile.lock(path, [opts], cb)

Acquire a file lock on the specified path

### lockFile.lockSync(path, [opts])

Acquire a file lock on the specified path

### lockFile.unlock(path, cb)

Close and unlink the lockfile.
...
```

#### <a name="apidoc.element.lockfile.unlock"></a>[function <span class="apidocSignatureSpan">lockfile.</span>unlock (path, cb)](#apidoc.element.lockfile.unlock)
- description and source-code
```javascript
unlock = function (path, cb) {
  debug('unlock', path)
  // best-effort.  unlocking an already-unlocked lock is a noop
  delete locks[path]
  fs.unlink(path, function (unlinkEr) { cb && cb() })
}
```
- example usage
```shell
...
lockFile.lock('some-file.lock', opts, function (er) {
  // if the er happens, then it failed to acquire a lock.
  // if there was not an error, then the file was created,
  // and won't be deleted until we unlock it.

  // do my stuff, free of interruptions
  // then, some time later, do:
  lockFile.unlock('some-file.lock', function (er) {
    // er means that an error happened, and is probably bad.
  })
})
'''

## Methods
...
```

#### <a name="apidoc.element.lockfile.unlockSync"></a>[function <span class="apidocSignatureSpan">lockfile.</span>unlockSync (path)](#apidoc.element.lockfile.unlockSync)
- description and source-code
```javascript
unlockSync = function (path) {
  debug('unlockSync', path)
  // best-effort.  unlocking an already-unlocked lock is a noop
  try { fs.unlinkSync(path) } catch (er) {}
  delete locks[path]
}
```
- example usage
```shell
...

Acquire a file lock on the specified path

### lockFile.unlock(path, cb)

Close and unlink the lockfile.

### lockFile.unlockSync(path)

Close and unlink the lockfile.

### lockFile.check(path, [opts], cb)

Check if the lockfile is locked and not stale.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
