# npmdoc-lockfile

#### basic api documentation for  [lockfile (v1.0.3)](https://github.com/npm/lockfile#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-lockfile.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-lockfile) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-lockfile.svg)](https://travis-ci.org/npmdoc/node-npmdoc-lockfile)

#### A very polite lock file utility, which endeavors to not litter, and to wait patiently for others.

[![NPM](https://nodei.co/npm/lockfile.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/lockfile)

- [https://npmdoc.github.io/node-npmdoc-lockfile/build/apidoc.html](https://npmdoc.github.io/node-npmdoc-lockfile/build/apidoc.html)

[![apidoc](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.buildCi.browser.%252Ftmp%252Fbuild%252Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-lockfile/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-lockfile/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Isaac Z. Schlueter",
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
            "name": "trevorburnham"
        },
        {
            "name": "isaacs"
        }
    ],
    "name": "lockfile",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/npm/lockfile.git"
    },
    "scripts": {
        "changelog": "bash gen-changelog.sh",
        "postversion": "npm run changelog && git add CHANGELOG.md && git commit -m 'update changelog - '${npm_package_version}",
        "test": "tap test/*.js --cov"
    },
    "version": "1.0.3",
    "bin": {}
}
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
